# Webpack5 설정하기 (ESM)


> 회사에서 Javascript 용 서비스 SDK를 개발하면서 설정했던 webpack5 설정파일을 기록한다. 오래전에 잠깐 접해보았던게 webpack2 버전이었는데 어느새 5까지 나왔다. 알고 사용하는게 아니다보니 쓰면서 애를 먹었다...🥺🥺🥺

# Webpack5 설정하기 (ESM 사용)

회사에서 개발한 SDK는 `CJS(CommonJS)`에서 `ESM(ECMAScript Module)` 모듈 방식을 사용하여 개발했다. `ESM` 모듈 로더 사용하기 위해 `package.json`에 아래 설정을 추가했다.

package.json
```json
"type": "module"
```

아래 글에서는 CJS가 기본값이기 때문에 라이브러리의 경우 CJS로 개발하는 것을 추천하고 있다.

- [Node Modules at War: Why CommonJS and ES Modules Can’t Get Along](https://redfin.engineering/node-modules-at-war-why-commonjs-and-es-modules-cant-get-along-9617135eeca1)


## 설치 라이브러리

```bash
npm install --save-dev webpack webpack-cli webpack-merge webpack-stream
```

- `webpack-merge` 
  + 웹팩 설정을 하나로 병합해주는 라이브러리. 웹팩 설정을 개발용과 배포용으로 나누기 위해 적용했다.
- [webpack-stream](https://webpack.kr/guides/integrations/#gulp)
  + 태스크 러너는 `gulp`를 사용하고 있다. gulp에서 웹팩을 모듈링작업을 추가 하기 위해 설치했다.

## webpack.config.js

```javascript
import webpack from "webpack";

const webpack5esmInteropRule = {
    test: /\.m?js/,
    resolve: {
        fullySpecified: false
    }
}

const config = {
    target: ['web', 'es5'],
    entry: {
        main: "./entries-webpack.js",
    },
    output: {
        filename: "tychain.min.js"
    },
    resolve: {
        modules: ['node_modules'],
        fallback: {
            assert: false,
            buffer: await Promise.resolve('buffer'),
            console: false,
            constants: false,
            crypto: await Promise.resolve('crypto-browserify'),
            domain: false,
            events: false,
            fs: false,
            http: false,
            https: false,
            os: false,
            path: false,
            punycode: false,
            process: false,
            querystring: false,
            stream: await Promise.resolve('stream-browserify'),
            string_decoder: false,
            sys: false,
            timers: false,
            tty: false,
            url: false,
            util: false,
            vm: false,
            zlib: false,
        }
    },
    module: {
        rules: [
            webpack5esmInteropRule,
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: "babel-loader",
                    options: {
                        presets: ["@babel/preset-env"],
                        plugins: ["@babel/plugin-proposal-class-properties"]
                    }
                }
            },
        ]
    },
    plugins: [
        new webpack.ProvidePlugin({
           Buffer: ['buffer', 'Buffer'],
           ndcrypto: ['crypto', 'ndcrypto'],

        }),
    ],
    optimization: {
        concatenateModules: true
    }
}
export default config;
```

### target
- 웹팩은 여러 환경과 타켓으로 컴파일 할 수 있다. 
- [Targets|webpack](https://webpack.js.org/configuration/target/)

### entry
- `entry`는 애플리케이션을 번들리할 때 프로세스를 시작할 지점이다.
- [Entry and Context|webpack](https://webpack.js.org/configuration/target/)

### output
- `output`은 컴파일 된 번들파일과 리소스를 어느 경로에 어떤 이름으로 생성할지 지정할 수 있다. `entry`는 여러 지점이 있을 수 있지만 `output`은 하나의 구성만 지정할 수 있다.
- [Output|webpack](https://webpack.js.org/concepts/output/)

### resolve
- `resolve` 옵션은 모듈을 해석하는 방식을 지정할 수 있다. 

#### resolve.modules
- 모듈을 해석할 때 검색할 디렉터리를 웹팩에 알려준다. 상대경로는 node가 현재 디렉터리와 상위 디렉터리를 통해 `node_modeuls`를 검색하는 방법과 유사하게 검색되며, 절대경로는 오직 주어진 디렉터리에서만 검색한다.

**webpack.config.js**
```javascript
module.exports = {
  //...
  resolve: {
    modules: ['node_modules'],
  },
};
```

**webpack.config.js**
```javascript
const path = require('path');

module.exports = {
  //...
  resolve: {
    modules: [path.resolve(__dirname, 'src'), 'node_modules'],
  },
};
```

#### resolve.fallback
webpack5에서는 Node.js 코어 모듈에 대해 자동 polyfill을 제공하지 않는다. 그러므로 브라우저에서 실행되는 코드에서 사용하는 경우 npm 모듈을 직접 설치하고 포함해야 한다. 

다음은 webpack 5 이전에 webpack이 사용한 polyfill 목록이다. 회사에서는 모든 설정파일도 ESM 방식으로 `webpack.conf.js` 파일 또한 ESM으로 작성되었다. ESM의 경우 아래와 같이 설정한다.

```javascript
crypto: await Promise.resolve('crypto-browserify'),
```


```javascript
resolve: {
    fallback: {
      assert: require.resolve('assert'),
      buffer: require.resolve('buffer'),
      console: require.resolve('console-browserify'),
      constants: require.resolve('constants-browserify'),
      crypto: require.resolve('crypto-browserify'),
      domain: require.resolve('domain-browser'),
      events: require.resolve('events'),
      http: require.resolve('stream-http'),
      https: require.resolve('https-browserify'),
      os: require.resolve('os-browserify/browser'),
      path: require.resolve('path-browserify'),
      punycode: require.resolve('punycode'),
      process: require.resolve('process/browser'),
      querystring: require.resolve('querystring-es3'),
      stream: require.resolve('stream-browserify'),
      string_decoder: require.resolve('string_decoder'),
      sys: require.resolve('util'),
      timers: require.resolve('timers-browserify'),
      tty: require.resolve('tty-browserify'),
      url: require.resolve('url'),
      util: require.resolve('util'),
      vm: require.resolve('vm-browserify'),
      zlib: require.resolve('browserify-zlib'),
    },
},
```




## To Do
- [ ] CJS, ESM 


## Reference
    
