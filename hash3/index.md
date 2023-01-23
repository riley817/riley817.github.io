# Javascript SHA3 라이브러리 조사


## 개요
Go의 라이브러리에서 사용하는 SHA3-512 해시값과 자바스립트에서 사용하는 CryptoJS의 SHA3-512 해시값이 다른 문제가 발생

## 테스트 대상 모듈
- [hash.js](https://www.npmjs.com/package/hash.js)
- [js-sha3](https://www.npmjs.com/package/js-sha3)
- [CrytoJS](https://www.npmjs.com/package/crypto-js)


## 테스트 방법
Go의 테스트 코드 작성 후 Go에서 생성한 해시값과 Javascript에서 위의 라이브러리로 생성했을 때 해시값이 같은지 비교한다.

## 테스트 결과
- hash.js : SHA3-512 관련 함수가 없는 것 같았다.
- js-sha3 : 성공
- CrytoJS : 실패

CrytoJS 의 경우 Go에서 사용하는 SHA3-512와 차이가 있는 것 같다. [cryptojs_document](https://cryptojs.gitbook.io/docs/)에 참고 사항이 있었다.

> NOTE: I made a mistake when I named this implementation SHA-3. It should be named Keccak[c=2d]. Each of the SHA-3 functions is based on an instance of the Keccak algorithm, which NIST selected as the winner of the SHA-3 competition, but those SHA-3 functions won't produce hashes identical to Keccak.

### Go 테스트 코드
```go
func TestSha3(t *testing.T) {

	addr, _ := utils.FromHex("079164CF59F8B91DA6EC0767CB78075D73E93994")
	tx, _ := utils.FromHex("D08BC04E9EDACA0D964BF7404CD1E9C4939C1F099F26177B9B8597BC521BC5CE")

	shashum1 := sha3.New512()
	shashum1.Write(addr)

	println("[addr to sha3-512] ", utils.ToHex(shashum1.Sum(nil), false))

	shashum2 := sha3.New512()
	shashum2.Write(tx)

	println("[txHash to sha3-512] ", utils.ToHex(shashum2.Sum(nil), false))

	hasher512 := sha3.New512()
	hasher512.Write(addr)
	hasher512.Write(tx)
	sha512 := hasher512.Sum(nil)

	hasher160 := ripemd160.New()
	_, _ = hasher160.Write(sha512)
	println("[data-account] ",  utils.ToHex(hasher160.Sum(nil), false))
	
}
```

### javascript
```javascript
test("Test sha3-512", () => {

    const u8Addr = Hex.toU8Array('079164CF59F8B91DA6EC0767CB78075D73E93994');
    const expected_sha3_512 = 'A6CEF82C523F10ABE2684E19FD1AB35580E8B2DCE4926280B6CEC829D2A1A734F46A642BAA5C62369F185577D23A8428D50C201842B62663DCEB1EF2CD911853';

    // js-sha3 (pass)
    const shasum = sha3_512.create()
    shasum.update(u8Addr)
    shasum.hex()
    expect(shasum.toString().toUpperCase()).toEqual(expected_sha3_512)

    // hashjs (fail)
    const shasum2 = hashjs.sha512().update(u8Addr).digest('hex')
    console.log(shasum2.toUpperCase())
    expect(shasum2.toUpperCase()).toEqual(expected_sha3_512)

    // CRYPTOJS (fail)
    const shasum3 = CryptoJS.SHA3.create(u8Addr, {outputLength: 512});
    expect(shasum3.toUpperCase()).toEqual(expected_sha3_512)

});
```

## 결론 및 답변 내용

SHA-3은 SHA-1과 SHA-2를 대체하기 위해 미국 국립표준기술연구소(NIST)가 공개적인 방식을 통해 선정한 암호화 해시 함수이다. 최종 적으로 `Keccak`이 SHA-3의 해시 알고리즘으로 선정되었고 그 이후 NIST에서 변경된 암호화 해시 함수 표준을 발표했다. 결과적으로 처음 선정되었던 `Keccak` 방식과 현재 공식적인 SHA3 방식이 일부 달라지게 되었다.

- CryptoJS 에서 구현한 SHA3은 공식적인 SHA3이 아니라 이전 SHA3(Keccak)으로 보임.
- `js-sha3` 라이브러리가 추가됨에 따라 최적화가 필요하므로 `hash-wasm` 라이브러리를 추천해주심.

