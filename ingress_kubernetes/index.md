# 

<!DOCTYPE html>
<!-- saved from url=(0067)https://kubernetes.io/ko/docs/concepts/services-networking/ingress/ -->
<html lang="ko" class="flip-nav"><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><style type="text/css">.swal-icon--error{border-color:#f27474;-webkit-animation:animateErrorIcon .5s;animation:animateErrorIcon .5s}.swal-icon--error__x-mark{position:relative;display:block;-webkit-animation:animateXMark .5s;animation:animateXMark .5s}.swal-icon--error__line{position:absolute;height:5px;width:47px;background-color:#f27474;display:block;top:37px;border-radius:2px}.swal-icon--error__line--left{-webkit-transform:rotate(45deg);transform:rotate(45deg);left:17px}.swal-icon--error__line--right{-webkit-transform:rotate(-45deg);transform:rotate(-45deg);right:16px}@-webkit-keyframes animateErrorIcon{0%{-webkit-transform:rotateX(100deg);transform:rotateX(100deg);opacity:0}to{-webkit-transform:rotateX(0deg);transform:rotateX(0deg);opacity:1}}@keyframes animateErrorIcon{0%{-webkit-transform:rotateX(100deg);transform:rotateX(100deg);opacity:0}to{-webkit-transform:rotateX(0deg);transform:rotateX(0deg);opacity:1}}@-webkit-keyframes animateXMark{0%{-webkit-transform:scale(.4);transform:scale(.4);margin-top:26px;opacity:0}50%{-webkit-transform:scale(.4);transform:scale(.4);margin-top:26px;opacity:0}80%{-webkit-transform:scale(1.15);transform:scale(1.15);margin-top:-6px}to{-webkit-transform:scale(1);transform:scale(1);margin-top:0;opacity:1}}@keyframes animateXMark{0%{-webkit-transform:scale(.4);transform:scale(.4);margin-top:26px;opacity:0}50%{-webkit-transform:scale(.4);transform:scale(.4);margin-top:26px;opacity:0}80%{-webkit-transform:scale(1.15);transform:scale(1.15);margin-top:-6px}to{-webkit-transform:scale(1);transform:scale(1);margin-top:0;opacity:1}}.swal-icon--warning{border-color:#f8bb86;-webkit-animation:pulseWarning .75s infinite alternate;animation:pulseWarning .75s infinite alternate}.swal-icon--warning__body{width:5px;height:47px;top:10px;border-radius:2px;margin-left:-2px}.swal-icon--warning__body,.swal-icon--warning__dot{position:absolute;left:50%;background-color:#f8bb86}.swal-icon--warning__dot{width:7px;height:7px;border-radius:50%;margin-left:-4px;bottom:-11px}@-webkit-keyframes pulseWarning{0%{border-color:#f8d486}to{border-color:#f8bb86}}@keyframes pulseWarning{0%{border-color:#f8d486}to{border-color:#f8bb86}}.swal-icon--success{border-color:#a5dc86}.swal-icon--success:after,.swal-icon--success:before{content:"";border-radius:50%;position:absolute;width:60px;height:120px;background:#fff;-webkit-transform:rotate(45deg);transform:rotate(45deg)}.swal-icon--success:before{border-radius:120px 0 0 120px;top:-7px;left:-33px;-webkit-transform:rotate(-45deg);transform:rotate(-45deg);-webkit-transform-origin:60px 60px;transform-origin:60px 60px}.swal-icon--success:after{border-radius:0 120px 120px 0;top:-11px;left:30px;-webkit-transform:rotate(-45deg);transform:rotate(-45deg);-webkit-transform-origin:0 60px;transform-origin:0 60px;-webkit-animation:rotatePlaceholder 4.25s ease-in;animation:rotatePlaceholder 4.25s ease-in}.swal-icon--success__ring{width:80px;height:80px;border:4px solid hsla(98,55%,69%,.2);border-radius:50%;box-sizing:content-box;position:absolute;left:-4px;top:-4px;z-index:2}.swal-icon--success__hide-corners{width:5px;height:90px;background-color:#fff;padding:1px;position:absolute;left:28px;top:8px;z-index:1;-webkit-transform:rotate(-45deg);transform:rotate(-45deg)}.swal-icon--success__line{height:5px;background-color:#a5dc86;display:block;border-radius:2px;position:absolute;z-index:2}.swal-icon--success__line--tip{width:25px;left:14px;top:46px;-webkit-transform:rotate(45deg);transform:rotate(45deg);-webkit-animation:animateSuccessTip .75s;animation:animateSuccessTip .75s}.swal-icon--success__line--long{width:47px;right:8px;top:38px;-webkit-transform:rotate(-45deg);transform:rotate(-45deg);-webkit-animation:animateSuccessLong .75s;animation:animateSuccessLong .75s}@-webkit-keyframes rotatePlaceholder{0%{-webkit-transform:rotate(-45deg);transform:rotate(-45deg)}5%{-webkit-transform:rotate(-45deg);transform:rotate(-45deg)}12%{-webkit-transform:rotate(-405deg);transform:rotate(-405deg)}to{-webkit-transform:rotate(-405deg);transform:rotate(-405deg)}}@keyframes rotatePlaceholder{0%{-webkit-transform:rotate(-45deg);transform:rotate(-45deg)}5%{-webkit-transform:rotate(-45deg);transform:rotate(-45deg)}12%{-webkit-transform:rotate(-405deg);transform:rotate(-405deg)}to{-webkit-transform:rotate(-405deg);transform:rotate(-405deg)}}@-webkit-keyframes animateSuccessTip{0%{width:0;left:1px;top:19px}54%{width:0;left:1px;top:19px}70%{width:50px;left:-8px;top:37px}84%{width:17px;left:21px;top:48px}to{width:25px;left:14px;top:45px}}@keyframes animateSuccessTip{0%{width:0;left:1px;top:19px}54%{width:0;left:1px;top:19px}70%{width:50px;left:-8px;top:37px}84%{width:17px;left:21px;top:48px}to{width:25px;left:14px;top:45px}}@-webkit-keyframes animateSuccessLong{0%{width:0;right:46px;top:54px}65%{width:0;right:46px;top:54px}84%{width:55px;right:0;top:35px}to{width:47px;right:8px;top:38px}}@keyframes animateSuccessLong{0%{width:0;right:46px;top:54px}65%{width:0;right:46px;top:54px}84%{width:55px;right:0;top:35px}to{width:47px;right:8px;top:38px}}.swal-icon--info{border-color:#c9dae1}.swal-icon--info:before{width:5px;height:29px;bottom:17px;border-radius:2px;margin-left:-2px}.swal-icon--info:after,.swal-icon--info:before{content:"";position:absolute;left:50%;background-color:#c9dae1}.swal-icon--info:after{width:7px;height:7px;border-radius:50%;margin-left:-3px;top:19px}.swal-icon{width:80px;height:80px;border-width:4px;border-style:solid;border-radius:50%;padding:0;position:relative;box-sizing:content-box;margin:20px auto}.swal-icon:first-child{margin-top:32px}.swal-icon--custom{width:auto;height:auto;max-width:100%;border:none;border-radius:0}.swal-icon img{max-width:100%;max-height:100%}.swal-title{color:rgba(0,0,0,.65);font-weight:600;text-transform:none;position:relative;display:block;padding:13px 16px;font-size:27px;line-height:normal;text-align:center;margin-bottom:0}.swal-title:first-child{margin-top:26px}.swal-title:not(:first-child){padding-bottom:0}.swal-title:not(:last-child){margin-bottom:13px}.swal-text{font-size:16px;position:relative;float:none;line-height:normal;vertical-align:top;text-align:left;display:inline-block;margin:0;padding:0 10px;font-weight:400;color:rgba(0,0,0,.64);max-width:calc(100% - 20px);overflow-wrap:break-word;box-sizing:border-box}.swal-text:first-child{margin-top:45px}.swal-text:last-child{margin-bottom:45px}.swal-footer{text-align:right;padding-top:13px;margin-top:13px;padding:13px 16px;border-radius:inherit;border-top-left-radius:0;border-top-right-radius:0}.swal-button-container{margin:5px;display:inline-block;position:relative}.swal-button{background-color:#7cd1f9;color:#fff;border:none;box-shadow:none;border-radius:5px;font-weight:600;font-size:14px;padding:10px 24px;margin:0;cursor:pointer}.swal-button:not([disabled]):hover{background-color:#78cbf2}.swal-button:active{background-color:#70bce0}.swal-button:focus{outline:none;box-shadow:0 0 0 1px #fff,0 0 0 3px rgba(43,114,165,.29)}.swal-button[disabled]{opacity:.5;cursor:default}.swal-button::-moz-focus-inner{border:0}.swal-button--cancel{color:#555;background-color:#efefef}.swal-button--cancel:not([disabled]):hover{background-color:#e8e8e8}.swal-button--cancel:active{background-color:#d7d7d7}.swal-button--cancel:focus{box-shadow:0 0 0 1px #fff,0 0 0 3px rgba(116,136,150,.29)}.swal-button--danger{background-color:#e64942}.swal-button--danger:not([disabled]):hover{background-color:#df4740}.swal-button--danger:active{background-color:#cf423b}.swal-button--danger:focus{box-shadow:0 0 0 1px #fff,0 0 0 3px rgba(165,43,43,.29)}.swal-content{padding:0 20px;margin-top:20px;font-size:medium}.swal-content:last-child{margin-bottom:20px}.swal-content__input,.swal-content__textarea{-webkit-appearance:none;background-color:#fff;border:none;font-size:14px;display:block;box-sizing:border-box;width:100%;border:1px solid rgba(0,0,0,.14);padding:10px 13px;border-radius:2px;transition:border-color .2s}.swal-content__input:focus,.swal-content__textarea:focus{outline:none;border-color:#6db8ff}.swal-content__textarea{resize:vertical}.swal-button--loading{color:transparent}.swal-button--loading~.swal-button__loader{opacity:1}.swal-button__loader{position:absolute;height:auto;width:43px;z-index:2;left:50%;top:50%;-webkit-transform:translateX(-50%) translateY(-50%);transform:translateX(-50%) translateY(-50%);text-align:center;pointer-events:none;opacity:0}.swal-button__loader div{display:inline-block;float:none;vertical-align:baseline;width:9px;height:9px;padding:0;border:none;margin:2px;opacity:.4;border-radius:7px;background-color:hsla(0,0%,100%,.9);transition:background .2s;-webkit-animation:swal-loading-anim 1s infinite;animation:swal-loading-anim 1s infinite}.swal-button__loader div:nth-child(3n+2){-webkit-animation-delay:.15s;animation-delay:.15s}.swal-button__loader div:nth-child(3n+3){-webkit-animation-delay:.3s;animation-delay:.3s}@-webkit-keyframes swal-loading-anim{0%{opacity:.4}20%{opacity:.4}50%{opacity:1}to{opacity:.4}}@keyframes swal-loading-anim{0%{opacity:.4}20%{opacity:.4}50%{opacity:1}to{opacity:.4}}.swal-overlay{position:fixed;top:0;bottom:0;left:0;right:0;text-align:center;font-size:0;overflow-y:auto;background-color:rgba(0,0,0,.4);z-index:10000;pointer-events:none;opacity:0;transition:opacity .3s}.swal-overlay:before{content:" ";display:inline-block;vertical-align:middle;height:100%}.swal-overlay--show-modal{opacity:1;pointer-events:auto}.swal-overlay--show-modal .swal-modal{opacity:1;pointer-events:auto;box-sizing:border-box;-webkit-animation:showSweetAlert .3s;animation:showSweetAlert .3s;will-change:transform}.swal-modal{width:478px;opacity:0;pointer-events:none;background-color:#fff;text-align:center;border-radius:5px;position:static;margin:20px auto;display:inline-block;vertical-align:middle;-webkit-transform:scale(1);transform:scale(1);-webkit-transform-origin:50% 50%;transform-origin:50% 50%;z-index:10001;transition:opacity .2s,-webkit-transform .3s;transition:transform .3s,opacity .2s;transition:transform .3s,opacity .2s,-webkit-transform .3s}@media (max-width:500px){.swal-modal{width:calc(100% - 20px)}}@-webkit-keyframes showSweetAlert{0%{-webkit-transform:scale(1);transform:scale(1)}1%{-webkit-transform:scale(.5);transform:scale(.5)}45%{-webkit-transform:scale(1.05);transform:scale(1.05)}80%{-webkit-transform:scale(.95);transform:scale(.95)}to{-webkit-transform:scale(1);transform:scale(1)}}@keyframes showSweetAlert{0%{-webkit-transform:scale(1);transform:scale(1)}1%{-webkit-transform:scale(.5);transform:scale(.5)}45%{-webkit-transform:scale(1.05);transform:scale(1.05)}80%{-webkit-transform:scale(.95);transform:scale(.95)}to{-webkit-transform:scale(1);transform:scale(1)}}</style>
<meta name="ROBOTS" content="INDEX, FOLLOW">
<script type="text/javascript" async="" src="./ingress_kubernetes_files/analytics.js"></script><script async="" src="./ingress_kubernetes_files/js"></script>
<script>window.dataLayer=window.dataLayer||[];function gtag(){dataLayer.push(arguments)}gtag('js',new Date),gtag('config','UA-36037335-10')</script>
<link rel="alternate" hreflang="en" href="https://kubernetes.io/docs/concepts/services-networking/ingress/">
<link rel="alternate" hreflang="zh" href="https://kubernetes.io/zh/docs/concepts/services-networking/ingress/">
<link rel="alternate" hreflang="ja" href="https://kubernetes.io/ja/docs/concepts/services-networking/ingress/">
<link rel="alternate" hreflang="fr" href="https://kubernetes.io/fr/docs/concepts/services-networking/ingress/">
<link rel="alternate" hreflang="id" href="https://kubernetes.io/id/docs/concepts/services-networking/ingress/">

<meta name="viewport" content="width=device-width,initial-scale=1,shrink-to-fit=no">
<meta name="generator" content="Hugo 0.87.0">
<link rel="shortcut icon" type="image/png" href="https://kubernetes.io/images/favicon.png">
<link rel="apple-touch-icon" href="https://kubernetes.io/favicons/apple-touch-icon-180x180.png" sizes="180x180">
<link rel="manifest" href="https://kubernetes.io/manifest.webmanifest">
<link rel="apple-touch-icon" href="https://kubernetes.io/images/kubernetes-192x192.png">
<title>인그레스(Ingress) | Kubernetes</title><meta property="og:title" content="인그레스(Ingress)">
<meta property="og:description" content="FEATURE STATE: Kubernetes v1.19 [stable]  클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.
인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.
용어 이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.
 노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신. 클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다. 에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터.">
<meta property="og:type" content="article">
<meta property="og:url" content="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/"><meta property="article:section" content="docs">
<meta property="article:modified_time" content="2022-03-25T10:37:49+09:00"><meta property="og:site_name" content="Kubernetes">
<meta itemprop="name" content="인그레스(Ingress)">
<meta itemprop="description" content="FEATURE STATE: Kubernetes v1.19 [stable]  클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.
인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.
용어 이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.
 노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신. 클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다. 에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터.">
<meta itemprop="dateModified" content="2022-03-25T10:37:49+09:00">
<meta itemprop="wordCount" content="2342">
<meta itemprop="keywords" content=""><meta name="twitter:card" content="summary">
<meta name="twitter:title" content="인그레스(Ingress)">
<meta name="twitter:description" content="FEATURE STATE: Kubernetes v1.19 [stable]  클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.
인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.
용어 이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.
 노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신. 클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다. 에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터.">
<script type="application/javascript">var doNotTrack=!1;doNotTrack||(window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)},ga.l=+new Date,ga('create','UA-00000000-0','auto'),ga('send','pageview'))</script>
<script async="" src="./ingress_kubernetes_files/analytics.js"></script>
<link rel="preload" href="./ingress_kubernetes_files/main.min.1de41d18aa0d4645d566fe3c8085a58701dd92a076a5c812222c4631fd3b9377.css" as="style">
<link href="./ingress_kubernetes_files/main.min.1de41d18aa0d4645d566fe3c8085a58701dd92a076a5c812222c4631fd3b9377.css" rel="stylesheet" integrity="">
<script src="./ingress_kubernetes_files/jquery-3.3.1.min.js" integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=" crossorigin="anonymous"></script>
<script type="application/ld+json">{"@context":"https://schema.org","@type":"Organization","url":"https://kubernetes.io","logo":"https://kubernetes.io/images/favicon.png","potentialAction":{"@type":"SearchAction","target":"https://kubernetes.io/search/?q={search_term_string}","query-input":"required name=search_term_string"}}</script>
<meta name="theme-color" content="#326ce5">
<link rel="stylesheet" href="./ingress_kubernetes_files/feature-states.css">
<meta name="description" content="FEATURE STATE: Kubernetes v1.19 [stable]  클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.
인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.
용어 이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.
 노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신. 클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다. 에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터.">
<meta property="og:description" content="FEATURE STATE: Kubernetes v1.19 [stable]  클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.
인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.
용어 이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.
 노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신. 클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다. 에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터.">
<meta name="twitter:description" content="FEATURE STATE: Kubernetes v1.19 [stable]  클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.
인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.
용어 이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.
 노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신. 클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다. 에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터.">
<meta property="og:url" content="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/">
<meta property="og:title" content="인그레스(Ingress)">
<meta name="twitter:title" content="인그레스(Ingress)">
<meta name="twitter:image" content="https://kubernetes.io/images/favicon.png">
<meta name="twitter:image:alt" content="Kubernetes">
<meta property="og:image" content="/images/kubernetes-horizontal-color.png">
<meta property="og:type" content="article">
<script async="" src="./ingress_kubernetes_files/mermaid.min.js"></script>
<script src="./ingress_kubernetes_files/script.js"></script>
<title>인그레스(Ingress) | Kubernetes</title>
</head>
<body class="td-page td-documentation">
<header>
<nav class="js-navbar-scroll navbar navbar-expand navbar-dark flex-column flex-md-row td-navbar" data-auto-burger="primary">
<a class="navbar-brand" href="https://kubernetes.io/ko/"></a>
<div class="td-navbar-nav-scroll ml-md-auto" id="main_navbar">
<ul class="navbar-nav mt-2 mt-lg-0">
<li class="nav-item mr-2 mb-lg-0">
<a class="nav-link active" href="https://kubernetes.io/ko/docs/">문서</a>
</li>
<li class="nav-item mr-2 mb-lg-0">
<a class="nav-link" href="https://kubernetes.io/ko/blog/">쿠버네티스 블로그</a>
</li>
<li class="nav-item mr-2 mb-lg-0">
<a class="nav-link" href="https://kubernetes.io/ko/training/">교육</a>
</li>
<li class="nav-item mr-2 mb-lg-0">
<a class="nav-link" href="https://kubernetes.io/ko/partners/">파트너</a>
</li>
<li class="nav-item mr-2 mb-lg-0">
<a class="nav-link" href="https://kubernetes.io/ko/community/">커뮤니티</a>
</li>
<li class="nav-item mr-2 mb-lg-0">
<a class="nav-link" href="https://kubernetes.io/ko/case-studies/">사례 연구</a>
</li>
<li class="nav-item dropdown">
<a class="nav-link dropdown-toggle" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
버전
</a>
<div class="dropdown-menu dropdown-menu-right" aria-labelledby="navbarDropdownMenuLink">
<a class="dropdown-item" href="https://kubernetes.io/releases">Release Information</a>
<a class="dropdown-item" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/">v1.24</a>
<a class="dropdown-item" href="https://v1-23.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/">v1.23</a>
<a class="dropdown-item" href="https://v1-22.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/">v1.22</a>
<a class="dropdown-item" href="https://v1-21.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/">v1.21</a>
<a class="dropdown-item" href="https://v1-20.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/">v1.20</a>
</div>
</li>
<li class="nav-item dropdown">
<a class="nav-link dropdown-toggle" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#" id="navbarDropdownMenuLink" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
한국어 Korean
</a>
<div class="dropdown-menu dropdown-menu-right" aria-labelledby="navbarDropdownMenuLink">
<a class="dropdown-item" href="https://kubernetes.io/docs/concepts/services-networking/ingress/">English</a>
<a class="dropdown-item" href="https://kubernetes.io/zh/docs/concepts/services-networking/ingress/">中文 Chinese</a>
<a class="dropdown-item" href="https://kubernetes.io/ja/docs/concepts/services-networking/ingress/">日本語 Japanese</a>
<a class="dropdown-item" href="https://kubernetes.io/fr/docs/concepts/services-networking/ingress/">Français</a>
<a class="dropdown-item" href="https://kubernetes.io/id/docs/concepts/services-networking/ingress/">Bahasa Indonesia</a>
</div>
</li>
</ul>
</div>
<button id="hamburger" onclick="kub.toggleMenu()" data-auto-burger-exclude=""><div></div></button>
</nav>
<div lang="en" id="announcement" style="background-color:#3371e3;color:#fff;background:#326ce5">
<aside>
<div class="announcement-main" data-nosnippet="">
<h4>
Dockershim removed from Kubernetes
</h4>
<p>As of Kubernetes 1.24, Dockershim is no longer included in Kubernetes.
Read the <a href="https://kubernetes.io/dockershim">Dockershim Removal FAQ</a> to see what this means to you.</p>
</div>
</aside>
</div>
<section class="header-hero filler bot-bar no-sub">
</section>
</header>
<div class="container-fluid td-outer">
<div class="td-main">
<div class="row flex-md-nowrap">
<div class="col-12 col-md-3 col-xl-2 td-sidebar d-print-none">
<script>$(function(){$("#td-section-nav a").removeClass("active"),$("#td-section-nav #m-ko-docs-concepts-services-networking-ingress").addClass("active"),$("#td-section-nav #m-ko-docs-concepts-services-networking-ingress-li span").addClass("td-sidebar-nav-active-item"),$("#td-section-nav #m-ko-docs-concepts-services-networking-ingress").parents("li").addClass("active-path"),$("#td-section-nav li.active-path").addClass("show"),$("#td-section-nav li.active-path").children("input").prop('checked',!0),$("#td-section-nav #m-ko-docs-concepts-services-networking-ingress-li").siblings("li").addClass("show"),$("#td-section-nav #m-ko-docs-concepts-services-networking-ingress-li").children("ul").children("li").addClass("show"),$("#td-sidebar-menu").toggleClass("d-none")})</script>
<div id="td-sidebar-menu" class="td-sidebar__inner">
<form class="td-sidebar__search d-flex align-items-center">
<input type="search" class="form-control td-search-input" name="q" placeholder=" 검색하기" aria-label="검색하기" autocomplete="off">
<button class="btn btn-link td-sidebar__toggle d-md-none p-0 ml-3 fas fa-bars" type="button" data-toggle="collapse" data-target="#td-section-nav" aria-controls="td-docs-nav" aria-expanded="false" aria-label="Toggle section navigation">
</button>
</form>
<nav class="collapse td-sidebar-nav foldable-nav" id="td-section-nav">
<ul class="td-sidebar-nav__section pr-md-3 ul-0">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child active-path show" id="m-ko-docs-li">
<ul class="ul-1">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-home-li">
<input type="checkbox" id="m-ko-docs-home-check">
<label for="m-ko-docs-home-check"><a href="https://kubernetes.io/ko/docs/home/" title="쿠버네티스 문서" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-home"><span>홈</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-home-supported-doc-versions-li">
<input type="checkbox" id="m-ko-docs-home-supported-doc-versions-check">
<label for="m-ko-docs-home-supported-doc-versions-check"><a href="https://kubernetes.io/ko/docs/home/supported-doc-versions/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-home-supported-doc-versions"><span>사용 가능한 문서의 버전</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-setup-li">
<input type="checkbox" id="m-ko-docs-setup-check">
<label for="m-ko-docs-setup-check"><a href="https://kubernetes.io/ko/docs/setup/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup"><span>시작하기</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-learning-environment-li">
<input type="checkbox" id="m-ko-docs-setup-learning-environment-check">
<label for="m-ko-docs-setup-learning-environment-check"><a href="https://kubernetes.io/ko/docs/setup/learning-environment/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup-learning-environment"><span>학습 환경</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-setup-production-environment-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-check">
<label for="m-ko-docs-setup-production-environment-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup-production-environment"><span>프로덕션 환경</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-container-runtimes-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-container-runtimes-check">
<label for="m-ko-docs-setup-production-environment-container-runtimes-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/container-runtimes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-container-runtimes"><span>컨테이너 런타임</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-setup-production-environment-tools-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-check">
<label for="m-ko-docs-setup-production-environment-tools-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup-production-environment-tools"><span>배포 도구로 쿠버네티스 설치하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-setup-production-environment-tools-kubeadm-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-kubeadm-check">
<label for="m-ko-docs-setup-production-environment-tools-kubeadm-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup-production-environment-tools-kubeadm"><span>kubeadm으로 클러스터 구성하기</span></a></label>
<ul class="ul-5 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-tools-kubeadm-install-kubeadm-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-kubeadm-install-kubeadm-check">
<label for="m-ko-docs-setup-production-environment-tools-kubeadm-install-kubeadm-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/install-kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-tools-kubeadm-install-kubeadm"><span>kubeadm 설치하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-production-environment-tools-kubeadm-troubleshooting-kubeadm-li">
<input type="checkbox" id="m-docs-setup-production-environment-tools-kubeadm-troubleshooting-kubeadm-check">
<label for="m-docs-setup-production-environment-tools-kubeadm-troubleshooting-kubeadm-check"><a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-production-environment-tools-kubeadm-troubleshooting-kubeadm"><span>Troubleshooting kubeadm</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-production-environment-tools-kubeadm-create-cluster-kubeadm-li">
<input type="checkbox" id="m-docs-setup-production-environment-tools-kubeadm-create-cluster-kubeadm-check">
<label for="m-docs-setup-production-environment-tools-kubeadm-create-cluster-kubeadm-check"><a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-production-environment-tools-kubeadm-create-cluster-kubeadm"><span>Creating a cluster with kubeadm</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-tools-kubeadm-control-plane-flags-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-kubeadm-control-plane-flags-check">
<label for="m-ko-docs-setup-production-environment-tools-kubeadm-control-plane-flags-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/control-plane-flags/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-tools-kubeadm-control-plane-flags"><span>kubeadm API로 컴포넌트 사용자 정의하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-tools-kubeadm-ha-topology-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-kubeadm-ha-topology-check">
<label for="m-ko-docs-setup-production-environment-tools-kubeadm-ha-topology-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/ha-topology/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-tools-kubeadm-ha-topology"><span>고가용성 토폴로지 선택</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-production-environment-tools-kubeadm-high-availability-li">
<input type="checkbox" id="m-docs-setup-production-environment-tools-kubeadm-high-availability-check">
<label for="m-docs-setup-production-environment-tools-kubeadm-high-availability-check"><a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-production-environment-tools-kubeadm-high-availability"><span>Creating Highly Available Clusters with kubeadm</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-production-environment-tools-kubeadm-setup-ha-etcd-with-kubeadm-li">
<input type="checkbox" id="m-docs-setup-production-environment-tools-kubeadm-setup-ha-etcd-with-kubeadm-check">
<label for="m-docs-setup-production-environment-tools-kubeadm-setup-ha-etcd-with-kubeadm-check"><a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-production-environment-tools-kubeadm-setup-ha-etcd-with-kubeadm"><span>Set up a High Availability etcd Cluster with kubeadm</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-production-environment-tools-kubeadm-kubelet-integration-li">
<input type="checkbox" id="m-docs-setup-production-environment-tools-kubeadm-kubelet-integration-check">
<label for="m-docs-setup-production-environment-tools-kubeadm-kubelet-integration-check"><a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/kubelet-integration/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-production-environment-tools-kubeadm-kubelet-integration"><span>Configuring each kubelet in your cluster using kubeadm</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-production-environment-tools-kubeadm-dual-stack-support-li">
<input type="checkbox" id="m-docs-setup-production-environment-tools-kubeadm-dual-stack-support-check">
<label for="m-docs-setup-production-environment-tools-kubeadm-dual-stack-support-check"><a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/dual-stack-support/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-production-environment-tools-kubeadm-dual-stack-support"><span>Dual-stack support with kubeadm</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-tools-kops-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-kops-check">
<label for="m-ko-docs-setup-production-environment-tools-kops-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/kops/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-tools-kops"><span>Kops로 쿠버네티스 설치하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-tools-kubespray-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-tools-kubespray-check">
<label for="m-ko-docs-setup-production-environment-tools-kubespray-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/tools/kubespray/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-tools-kubespray"><span>Kubespray로 쿠버네티스 설치하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-turnkey-solutions-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-turnkey-solutions-check">
<label for="m-ko-docs-setup-production-environment-turnkey-solutions-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/turnkey-solutions/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-turnkey-solutions"><span>턴키 클라우드 솔루션</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-setup-production-environment-windows-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-windows-check">
<label for="m-ko-docs-setup-production-environment-windows-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/windows/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup-production-environment-windows"><span>쿠버네티스에서 윈도우</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-windows-intro-windows-in-kubernetes-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-windows-intro-windows-in-kubernetes-check">
<label for="m-ko-docs-setup-production-environment-windows-intro-windows-in-kubernetes-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/windows/intro-windows-in-kubernetes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-windows-intro-windows-in-kubernetes"><span>쿠버네티스에서 윈도우 컨테이너</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-production-environment-windows-user-guide-windows-containers-li">
<input type="checkbox" id="m-ko-docs-setup-production-environment-windows-user-guide-windows-containers-check">
<label for="m-ko-docs-setup-production-environment-windows-user-guide-windows-containers-check"><a href="https://kubernetes.io/ko/docs/setup/production-environment/windows/user-guide-windows-containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-production-environment-windows-user-guide-windows-containers"><span>쿠버네티스에서 윈도우 컨테이너 스케줄링을 위한 가이드</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-setup-best-practices-li">
<input type="checkbox" id="m-ko-docs-setup-best-practices-check">
<label for="m-ko-docs-setup-best-practices-check"><a href="https://kubernetes.io/ko/docs/setup/best-practices/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-setup-best-practices"><span>모범 사례</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-best-practices-cluster-large-li">
<input type="checkbox" id="m-ko-docs-setup-best-practices-cluster-large-check">
<label for="m-ko-docs-setup-best-practices-cluster-large-check"><a href="https://kubernetes.io/ko/docs/setup/best-practices/cluster-large/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-best-practices-cluster-large"><span>대형 클러스터에 대한 고려 사항</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-best-practices-multiple-zones-li">
<input type="checkbox" id="m-ko-docs-setup-best-practices-multiple-zones-check">
<label for="m-ko-docs-setup-best-practices-multiple-zones-check"><a href="https://kubernetes.io/ko/docs/setup/best-practices/multiple-zones/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-best-practices-multiple-zones"><span>여러 영역에서 실행</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-best-practices-node-conformance-li">
<input type="checkbox" id="m-ko-docs-setup-best-practices-node-conformance-check">
<label for="m-ko-docs-setup-best-practices-node-conformance-check"><a href="https://kubernetes.io/ko/docs/setup/best-practices/node-conformance/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-best-practices-node-conformance"><span>노드 구성 검증하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-setup-best-practices-enforcing-pod-security-standards-li">
<input type="checkbox" id="m-docs-setup-best-practices-enforcing-pod-security-standards-check">
<label for="m-docs-setup-best-practices-enforcing-pod-security-standards-check"><a href="https://kubernetes.io/docs/setup/best-practices/enforcing-pod-security-standards/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-setup-best-practices-enforcing-pod-security-standards"><span>Enforcing Pod Security Standards</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-setup-best-practices-certificates-li">
<input type="checkbox" id="m-ko-docs-setup-best-practices-certificates-check">
<label for="m-ko-docs-setup-best-practices-certificates-check"><a href="https://kubernetes.io/ko/docs/setup/best-practices/certificates/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-setup-best-practices-certificates"><span>PKI 인증서 및 요구 사항</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child active-path show" id="m-ko-docs-concepts-li">
<input type="checkbox" id="m-ko-docs-concepts-check">
<label for="m-ko-docs-concepts-check"><a href="https://kubernetes.io/ko/docs/concepts/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts"><span>개념</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-overview-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-check">
<label for="m-ko-docs-concepts-overview-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-overview"><span>개요</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-what-is-kubernetes-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-what-is-kubernetes-check">
<label for="m-ko-docs-concepts-overview-what-is-kubernetes-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-what-is-kubernetes"><span>쿠버네티스란 무엇인가?</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-components-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-components-check">
<label for="m-ko-docs-concepts-overview-components-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/components/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-components"><span>쿠버네티스 컴포넌트</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-kubernetes-api-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-kubernetes-api-check">
<label for="m-ko-docs-concepts-overview-kubernetes-api-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/kubernetes-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-kubernetes-api"><span>쿠버네티스 API</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-overview-working-with-objects-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-overview-working-with-objects"><span>쿠버네티스 오브젝트로 작업하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-kubernetes-objects-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-kubernetes-objects-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-kubernetes-objects-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/kubernetes-objects/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-kubernetes-objects"><span>쿠버네티스 오브젝트 이해하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-object-management-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-object-management-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-object-management-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/object-management/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-object-management"><span>쿠버네티스 오브젝트 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-names-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-names-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-names-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/names/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-names"><span>오브젝트 이름과 ID</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-namespaces-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-namespaces-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-namespaces-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/namespaces/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-namespaces"><span>네임스페이스</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-labels-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-labels-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-labels-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/labels/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-labels"><span>레이블과 셀렉터</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-annotations-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-annotations-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-annotations-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/annotations/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-annotations"><span>어노테이션</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-overview-working-with-objects-finalizers-li">
<input type="checkbox" id="m-docs-concepts-overview-working-with-objects-finalizers-check">
<label for="m-docs-concepts-overview-working-with-objects-finalizers-check"><a href="https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-overview-working-with-objects-finalizers"><span>Finalizers</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-overview-working-with-objects-owners-dependents-li">
<input type="checkbox" id="m-docs-concepts-overview-working-with-objects-owners-dependents-check">
<label for="m-docs-concepts-overview-working-with-objects-owners-dependents-check"><a href="https://kubernetes.io/docs/concepts/overview/working-with-objects/owners-dependents/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-overview-working-with-objects-owners-dependents"><span>Owners and Dependents</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-field-selectors-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-field-selectors-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-field-selectors-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/field-selectors/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-field-selectors"><span>필드 셀렉터</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-overview-working-with-objects-common-labels-li">
<input type="checkbox" id="m-ko-docs-concepts-overview-working-with-objects-common-labels-check">
<label for="m-ko-docs-concepts-overview-working-with-objects-common-labels-check"><a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/common-labels/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-overview-working-with-objects-common-labels"><span>권장 레이블</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-architecture-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-check">
<label for="m-ko-docs-concepts-architecture-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-architecture"><span>클러스터 아키텍처</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-architecture-nodes-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-nodes-check">
<label for="m-ko-docs-concepts-architecture-nodes-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/nodes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-architecture-nodes"><span>노드</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-architecture-control-plane-node-communication-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-control-plane-node-communication-check">
<label for="m-ko-docs-concepts-architecture-control-plane-node-communication-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/control-plane-node-communication/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-architecture-control-plane-node-communication"><span>컨트롤 플레인-노드 간 통신</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-architecture-controller-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-controller-check">
<label for="m-ko-docs-concepts-architecture-controller-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/controller/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-architecture-controller"><span>컨트롤러</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-architecture-cloud-controller-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-cloud-controller-check">
<label for="m-ko-docs-concepts-architecture-cloud-controller-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/cloud-controller/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-architecture-cloud-controller"><span>클라우드 컨트롤러 매니저</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-architecture-garbage-collection-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-garbage-collection-check">
<label for="m-ko-docs-concepts-architecture-garbage-collection-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/garbage-collection/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-architecture-garbage-collection"><span>가비지(Garbage) 수집</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-architecture-cri-li">
<input type="checkbox" id="m-ko-docs-concepts-architecture-cri-check">
<label for="m-ko-docs-concepts-architecture-cri-check"><a href="https://kubernetes.io/ko/docs/concepts/architecture/cri/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-architecture-cri"><span>컨테이너 런타임 인터페이스(CRI)</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-containers-li">
<input type="checkbox" id="m-ko-docs-concepts-containers-check">
<label for="m-ko-docs-concepts-containers-check"><a href="https://kubernetes.io/ko/docs/concepts/containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-containers"><span>컨테이너</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-containers-images-li">
<input type="checkbox" id="m-ko-docs-concepts-containers-images-check">
<label for="m-ko-docs-concepts-containers-images-check"><a href="https://kubernetes.io/ko/docs/concepts/containers/images/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-containers-images"><span>이미지</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-containers-runtime-class-li">
<input type="checkbox" id="m-ko-docs-concepts-containers-runtime-class-check">
<label for="m-ko-docs-concepts-containers-runtime-class-check"><a href="https://kubernetes.io/ko/docs/concepts/containers/runtime-class/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-containers-runtime-class"><span>런타임클래스(RuntimeClass)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-containers-container-environment-li">
<input type="checkbox" id="m-ko-docs-concepts-containers-container-environment-check">
<label for="m-ko-docs-concepts-containers-container-environment-check"><a href="https://kubernetes.io/ko/docs/concepts/containers/container-environment/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-containers-container-environment"><span>컨테이너 환경 변수</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-containers-container-lifecycle-hooks-li">
<input type="checkbox" id="m-ko-docs-concepts-containers-container-lifecycle-hooks-check">
<label for="m-ko-docs-concepts-containers-container-lifecycle-hooks-check"><a href="https://kubernetes.io/ko/docs/concepts/containers/container-lifecycle-hooks/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-containers-container-lifecycle-hooks"><span>컨테이너 라이프사이클 훅(Hook)</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-workloads-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-check">
<label for="m-ko-docs-concepts-workloads-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-workloads"><span>워크로드</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-workloads-pods-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-pods-check">
<label for="m-ko-docs-concepts-workloads-pods-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-workloads-pods"><span>파드</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-pods-pod-lifecycle-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-pods-pod-lifecycle-check">
<label for="m-ko-docs-concepts-workloads-pods-pod-lifecycle-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/pod-lifecycle/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-pods-pod-lifecycle"><span>파드 라이프사이클</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-pods-init-containers-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-pods-init-containers-check">
<label for="m-ko-docs-concepts-workloads-pods-init-containers-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-pods-init-containers"><span>초기화 컨테이너</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-pods-pod-topology-spread-constraints-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-pods-pod-topology-spread-constraints-check">
<label for="m-ko-docs-concepts-workloads-pods-pod-topology-spread-constraints-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/pod-topology-spread-constraints/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-pods-pod-topology-spread-constraints"><span>파드 토폴로지 분배 제약 조건</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-pods-disruptions-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-pods-disruptions-check">
<label for="m-ko-docs-concepts-workloads-pods-disruptions-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/disruptions/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-pods-disruptions"><span>중단(disruption)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-pods-ephemeral-containers-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-pods-ephemeral-containers-check">
<label for="m-ko-docs-concepts-workloads-pods-ephemeral-containers-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/ephemeral-containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-pods-ephemeral-containers"><span>임시(Ephemeral) 컨테이너</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-workloads-controllers-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-check">
<label for="m-ko-docs-concepts-workloads-controllers-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-workloads-controllers"><span>워크로드 리소스</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-deployment-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-deployment-check">
<label for="m-ko-docs-concepts-workloads-controllers-deployment-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-deployment"><span>디플로이먼트</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-replicaset-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-replicaset-check">
<label for="m-ko-docs-concepts-workloads-controllers-replicaset-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/replicaset/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-replicaset"><span>레플리카셋</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-statefulset-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-statefulset-check">
<label for="m-ko-docs-concepts-workloads-controllers-statefulset-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/statefulset/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-statefulset"><span>스테이트풀셋</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-daemonset-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-daemonset-check">
<label for="m-ko-docs-concepts-workloads-controllers-daemonset-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/daemonset/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-daemonset"><span>데몬셋</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-job-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-job-check">
<label for="m-ko-docs-concepts-workloads-controllers-job-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/job/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-job"><span>잡</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-ttlafterfinished-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-ttlafterfinished-check">
<label for="m-ko-docs-concepts-workloads-controllers-ttlafterfinished-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/ttlafterfinished/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-ttlafterfinished"><span>완료된 잡 자동 정리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-cron-jobs-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-cron-jobs-check">
<label for="m-ko-docs-concepts-workloads-controllers-cron-jobs-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/cron-jobs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-cron-jobs"><span>크론잡</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-workloads-controllers-replicationcontroller-li">
<input type="checkbox" id="m-ko-docs-concepts-workloads-controllers-replicationcontroller-check">
<label for="m-ko-docs-concepts-workloads-controllers-replicationcontroller-check"><a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/replicationcontroller/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-workloads-controllers-replicationcontroller"><span>레플리케이션 컨트롤러</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child active-path show" id="m-ko-docs-concepts-services-networking-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-check">
<label for="m-ko-docs-concepts-services-networking-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-services-networking"><span>서비스, 로드밸런싱, 네트워킹</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-service-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-service-check">
<label for="m-ko-docs-concepts-services-networking-service-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-service"><span>서비스</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-service-topology-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-service-topology-check">
<label for="m-ko-docs-concepts-services-networking-service-topology-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/service-topology/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-service-topology"><span>토폴로지 키를 사용하여 토폴로지-인지 트래픽 라우팅</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-dns-pod-service-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-dns-pod-service-check">
<label for="m-ko-docs-concepts-services-networking-dns-pod-service-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/dns-pod-service/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-dns-pod-service"><span>서비스 및 파드용 DNS</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-connect-applications-service-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-connect-applications-service-check">
<label for="m-ko-docs-concepts-services-networking-connect-applications-service-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/connect-applications-service/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-connect-applications-service"><span>서비스와 애플리케이션 연결하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-ingress-controllers-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-ingress-controllers-check">
<label for="m-ko-docs-concepts-services-networking-ingress-controllers-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-ingress-controllers"><span>인그레스 컨트롤러</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child active-path show" id="m-ko-docs-concepts-services-networking-ingress-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-ingress-check">
<label for="m-ko-docs-concepts-services-networking-ingress-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page active" id="m-ko-docs-concepts-services-networking-ingress"><span class="td-sidebar-nav-active-item">인그레스(Ingress)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-service-traffic-policy-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-service-traffic-policy-check">
<label for="m-ko-docs-concepts-services-networking-service-traffic-policy-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/service-traffic-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-service-traffic-policy"><span>서비스 내부 트래픽 정책</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-endpoint-slices-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-endpoint-slices-check">
<label for="m-ko-docs-concepts-services-networking-endpoint-slices-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/endpoint-slices/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-endpoint-slices"><span>엔드포인트슬라이스</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-topology-aware-hints-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-topology-aware-hints-check">
<label for="m-ko-docs-concepts-services-networking-topology-aware-hints-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/topology-aware-hints/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-topology-aware-hints"><span>토폴로지 인지 힌트</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-network-policies-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-network-policies-check">
<label for="m-ko-docs-concepts-services-networking-network-policies-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/network-policies/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-network-policies"><span>네트워크 정책</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child show" id="m-ko-docs-concepts-services-networking-dual-stack-li">
<input type="checkbox" id="m-ko-docs-concepts-services-networking-dual-stack-check">
<label for="m-ko-docs-concepts-services-networking-dual-stack-check"><a href="https://kubernetes.io/ko/docs/concepts/services-networking/dual-stack/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-services-networking-dual-stack"><span>IPv4/IPv6 이중 스택</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-storage-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-check">
<label for="m-ko-docs-concepts-storage-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-storage"><span>스토리지</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-volumes-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-volumes-check">
<label for="m-ko-docs-concepts-storage-volumes-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/volumes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-volumes"><span>볼륨</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-persistent-volumes-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-persistent-volumes-check">
<label for="m-ko-docs-concepts-storage-persistent-volumes-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/persistent-volumes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-persistent-volumes"><span>퍼시스턴트 볼륨</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-storage-projected-volumes-li">
<input type="checkbox" id="m-docs-concepts-storage-projected-volumes-check">
<label for="m-docs-concepts-storage-projected-volumes-check"><a href="https://kubernetes.io/docs/concepts/storage/projected-volumes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-storage-projected-volumes"><span>Projected Volumes</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-storage-classes-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-storage-classes-check">
<label for="m-ko-docs-concepts-storage-storage-classes-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/storage-classes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-storage-classes"><span>스토리지 클래스</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-ephemeral-volumes-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-ephemeral-volumes-check">
<label for="m-ko-docs-concepts-storage-ephemeral-volumes-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/ephemeral-volumes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-ephemeral-volumes"><span>임시 볼륨</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-dynamic-provisioning-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-dynamic-provisioning-check">
<label for="m-ko-docs-concepts-storage-dynamic-provisioning-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/dynamic-provisioning/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-dynamic-provisioning"><span>동적 볼륨 프로비저닝</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-volume-snapshots-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-volume-snapshots-check">
<label for="m-ko-docs-concepts-storage-volume-snapshots-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/volume-snapshots/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-volume-snapshots"><span>볼륨 스냅샷</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-volume-snapshot-classes-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-volume-snapshot-classes-check">
<label for="m-ko-docs-concepts-storage-volume-snapshot-classes-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/volume-snapshot-classes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-volume-snapshot-classes"><span>볼륨 스냅샷 클래스</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-volume-pvc-datasource-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-volume-pvc-datasource-check">
<label for="m-ko-docs-concepts-storage-volume-pvc-datasource-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/volume-pvc-datasource/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-volume-pvc-datasource"><span>CSI 볼륨 복제하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-storage-capacity-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-storage-capacity-check">
<label for="m-ko-docs-concepts-storage-storage-capacity-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/storage-capacity/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-storage-capacity"><span>스토리지 용량</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-storage-limits-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-storage-limits-check">
<label for="m-ko-docs-concepts-storage-storage-limits-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/storage-limits/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-storage-limits"><span>노드 별 볼륨 한도</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-storage-volume-health-monitoring-li">
<input type="checkbox" id="m-ko-docs-concepts-storage-volume-health-monitoring-check">
<label for="m-ko-docs-concepts-storage-volume-health-monitoring-check"><a href="https://kubernetes.io/ko/docs/concepts/storage/volume-health-monitoring/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-storage-volume-health-monitoring"><span>볼륨 헬스 모니터링</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-configuration-li">
<input type="checkbox" id="m-ko-docs-concepts-configuration-check">
<label for="m-ko-docs-concepts-configuration-check"><a href="https://kubernetes.io/ko/docs/concepts/configuration/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-configuration"><span>구성</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-configuration-overview-li">
<input type="checkbox" id="m-ko-docs-concepts-configuration-overview-check">
<label for="m-ko-docs-concepts-configuration-overview-check"><a href="https://kubernetes.io/ko/docs/concepts/configuration/overview/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-configuration-overview"><span>구성 모범 사례</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-configuration-configmap-li">
<input type="checkbox" id="m-ko-docs-concepts-configuration-configmap-check">
<label for="m-ko-docs-concepts-configuration-configmap-check"><a href="https://kubernetes.io/ko/docs/concepts/configuration/configmap/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-configuration-configmap"><span>컨피그맵(ConfigMap)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-configuration-secret-li">
<input type="checkbox" id="m-ko-docs-concepts-configuration-secret-check">
<label for="m-ko-docs-concepts-configuration-secret-check"><a href="https://kubernetes.io/ko/docs/concepts/configuration/secret/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-configuration-secret"><span>시크릿(Secret)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-configuration-manage-resources-containers-li">
<input type="checkbox" id="m-ko-docs-concepts-configuration-manage-resources-containers-check">
<label for="m-ko-docs-concepts-configuration-manage-resources-containers-check"><a href="https://kubernetes.io/ko/docs/concepts/configuration/manage-resources-containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-configuration-manage-resources-containers"><span>파드 및 컨테이너 리소스 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-configuration-organize-cluster-access-kubeconfig-li">
<input type="checkbox" id="m-ko-docs-concepts-configuration-organize-cluster-access-kubeconfig-check">
<label for="m-ko-docs-concepts-configuration-organize-cluster-access-kubeconfig-check"><a href="https://kubernetes.io/ko/docs/concepts/configuration/organize-cluster-access-kubeconfig/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-configuration-organize-cluster-access-kubeconfig"><span>kubeconfig 파일을 사용하여 클러스터 접근 구성하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-configuration-windows-resource-management-li">
<input type="checkbox" id="m-docs-concepts-configuration-windows-resource-management-check">
<label for="m-docs-concepts-configuration-windows-resource-management-check"><a href="https://kubernetes.io/docs/concepts/configuration/windows-resource-management/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-configuration-windows-resource-management"><span>Resource Management for Windows nodes</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-security-li">
<input type="checkbox" id="m-ko-docs-concepts-security-check">
<label for="m-ko-docs-concepts-security-check"><a href="https://kubernetes.io/ko/docs/concepts/security/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-security"><span>보안</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-security-overview-li">
<input type="checkbox" id="m-ko-docs-concepts-security-overview-check">
<label for="m-ko-docs-concepts-security-overview-check"><a href="https://kubernetes.io/ko/docs/concepts/security/overview/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-security-overview"><span>클라우드 네이티브 보안 개요</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-security-controlling-access-li">
<input type="checkbox" id="m-ko-docs-concepts-security-controlling-access-check">
<label for="m-ko-docs-concepts-security-controlling-access-check"><a href="https://kubernetes.io/ko/docs/concepts/security/controlling-access/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-security-controlling-access"><span>쿠버네티스 API 접근 제어하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-security-pod-security-standards-li">
<input type="checkbox" id="m-docs-concepts-security-pod-security-standards-check">
<label for="m-docs-concepts-security-pod-security-standards-check"><a href="https://kubernetes.io/docs/concepts/security/pod-security-standards/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-security-pod-security-standards"><span>Pod Security Standards</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-security-pod-security-admission-li">
<input type="checkbox" id="m-docs-concepts-security-pod-security-admission-check">
<label for="m-docs-concepts-security-pod-security-admission-check"><a href="https://kubernetes.io/docs/concepts/security/pod-security-admission/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-security-pod-security-admission"><span>Pod Security Admission</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-security-pod-security-policy-li">
<input type="checkbox" id="m-docs-concepts-security-pod-security-policy-check">
<label for="m-docs-concepts-security-pod-security-policy-check"><a href="https://kubernetes.io/docs/concepts/security/pod-security-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-security-pod-security-policy"><span>Pod Security Policies</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-security-windows-security-li">
<input type="checkbox" id="m-docs-concepts-security-windows-security-check">
<label for="m-docs-concepts-security-windows-security-check"><a href="https://kubernetes.io/docs/concepts/security/windows-security/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-security-windows-security"><span>Security For Windows Nodes</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-scheduling-eviction-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-check">
<label for="m-ko-docs-concepts-scheduling-eviction-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-scheduling-eviction"><span>스케줄링, 선점(Preemption), 축출(Eviction)</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-kube-scheduler-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-kube-scheduler-check">
<label for="m-ko-docs-concepts-scheduling-eviction-kube-scheduler-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/kube-scheduler/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-kube-scheduler"><span>쿠버네티스 스케줄러</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-assign-pod-node-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-assign-pod-node-check">
<label for="m-ko-docs-concepts-scheduling-eviction-assign-pod-node-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/assign-pod-node/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-assign-pod-node"><span>노드에 파드 할당하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-pod-overhead-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-pod-overhead-check">
<label for="m-ko-docs-concepts-scheduling-eviction-pod-overhead-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/pod-overhead/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-pod-overhead"><span>파드 오버헤드</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-taint-and-toleration-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-taint-and-toleration-check">
<label for="m-ko-docs-concepts-scheduling-eviction-taint-and-toleration-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/taint-and-toleration/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-taint-and-toleration"><span>테인트(Taints)와 톨러레이션(Tolerations)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-node-pressure-eviction-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-node-pressure-eviction-check">
<label for="m-ko-docs-concepts-scheduling-eviction-node-pressure-eviction-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/node-pressure-eviction/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-node-pressure-eviction"><span>노드-압박 축출</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-api-eviction-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-api-eviction-check">
<label for="m-ko-docs-concepts-scheduling-eviction-api-eviction-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/api-eviction/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-api-eviction"><span>API를 이용한 축출(Eviction)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-pod-priority-preemption-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-pod-priority-preemption-check">
<label for="m-ko-docs-concepts-scheduling-eviction-pod-priority-preemption-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/pod-priority-preemption/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-pod-priority-preemption"><span>파드 우선순위(priority)와 선점(preemption)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-resource-bin-packing-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-resource-bin-packing-check">
<label for="m-ko-docs-concepts-scheduling-eviction-resource-bin-packing-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/resource-bin-packing/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-resource-bin-packing"><span>확장된 리소스를 위한 리소스 빈 패킹(bin packing)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-scheduling-eviction-scheduling-framework-li">
<input type="checkbox" id="m-docs-concepts-scheduling-eviction-scheduling-framework-check">
<label for="m-docs-concepts-scheduling-eviction-scheduling-framework-check"><a href="https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-scheduling-eviction-scheduling-framework"><span>Scheduling Framework</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-scheduling-eviction-scheduler-perf-tuning-li">
<input type="checkbox" id="m-ko-docs-concepts-scheduling-eviction-scheduler-perf-tuning-check">
<label for="m-ko-docs-concepts-scheduling-eviction-scheduler-perf-tuning-check"><a href="https://kubernetes.io/ko/docs/concepts/scheduling-eviction/scheduler-perf-tuning/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-scheduling-eviction-scheduler-perf-tuning"><span>스케줄러 성능 튜닝</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-policy-li">
<input type="checkbox" id="m-ko-docs-concepts-policy-check">
<label for="m-ko-docs-concepts-policy-check"><a href="https://kubernetes.io/ko/docs/concepts/policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-policy"><span>정책</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-policy-limit-range-li">
<input type="checkbox" id="m-ko-docs-concepts-policy-limit-range-check">
<label for="m-ko-docs-concepts-policy-limit-range-check"><a href="https://kubernetes.io/ko/docs/concepts/policy/limit-range/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-policy-limit-range"><span>리밋 레인지(Limit Range)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-policy-resource-quotas-li">
<input type="checkbox" id="m-ko-docs-concepts-policy-resource-quotas-check">
<label for="m-ko-docs-concepts-policy-resource-quotas-check"><a href="https://kubernetes.io/ko/docs/concepts/policy/resource-quotas/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-policy-resource-quotas"><span>리소스 쿼터</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-policy-pod-security-policy-li">
<input type="checkbox" id="m-ko-docs-concepts-policy-pod-security-policy-check">
<label for="m-ko-docs-concepts-policy-pod-security-policy-check"><a href="https://kubernetes.io/ko/docs/concepts/policy/pod-security-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-policy-pod-security-policy"><span>파드 시큐리티 폴리시</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-policy-pid-limiting-li">
<input type="checkbox" id="m-docs-concepts-policy-pid-limiting-check">
<label for="m-docs-concepts-policy-pid-limiting-check"><a href="https://kubernetes.io/docs/concepts/policy/pid-limiting/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-policy-pid-limiting"><span>Process ID Limits And Reservations</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-policy-node-resource-managers-li">
<input type="checkbox" id="m-ko-docs-concepts-policy-node-resource-managers-check">
<label for="m-ko-docs-concepts-policy-node-resource-managers-check"><a href="https://kubernetes.io/ko/docs/concepts/policy/node-resource-managers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-policy-node-resource-managers"><span>노드 리소스 매니저</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-cluster-administration-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-check">
<label for="m-ko-docs-concepts-cluster-administration-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-cluster-administration"><span>클러스터 관리</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-certificates-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-certificates-check">
<label for="m-ko-docs-concepts-cluster-administration-certificates-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/certificates/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-certificates"><span>인증서</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-manage-deployment-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-manage-deployment-check">
<label for="m-ko-docs-concepts-cluster-administration-manage-deployment-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/manage-deployment/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-manage-deployment"><span>리소스 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-networking-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-networking-check">
<label for="m-ko-docs-concepts-cluster-administration-networking-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/networking/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-networking"><span>클러스터 네트워킹</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-cluster-administration-system-traces-li">
<input type="checkbox" id="m-docs-concepts-cluster-administration-system-traces-check">
<label for="m-docs-concepts-cluster-administration-system-traces-check"><a href="https://kubernetes.io/docs/concepts/cluster-administration/system-traces/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-cluster-administration-system-traces"><span>Traces For Kubernetes System Components</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-logging-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-logging-check">
<label for="m-ko-docs-concepts-cluster-administration-logging-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/logging/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-logging"><span>로깅 아키텍처</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-system-logs-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-system-logs-check">
<label for="m-ko-docs-concepts-cluster-administration-system-logs-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/system-logs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-system-logs"><span>시스템 로그</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-system-metrics-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-system-metrics-check">
<label for="m-ko-docs-concepts-cluster-administration-system-metrics-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/system-metrics/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-system-metrics"><span>쿠버네티스 시스템 컴포넌트에 대한 메트릭</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-proxies-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-proxies-check">
<label for="m-ko-docs-concepts-cluster-administration-proxies-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/proxies/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-proxies"><span>쿠버네티스에서 프락시(Proxy)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-concepts-cluster-administration-flow-control-li">
<input type="checkbox" id="m-docs-concepts-cluster-administration-flow-control-check">
<label for="m-docs-concepts-cluster-administration-flow-control-check"><a href="https://kubernetes.io/docs/concepts/cluster-administration/flow-control/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-concepts-cluster-administration-flow-control"><span>API Priority and Fairness</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-cluster-administration-addons-li">
<input type="checkbox" id="m-ko-docs-concepts-cluster-administration-addons-check">
<label for="m-ko-docs-concepts-cluster-administration-addons-check"><a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/addons/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-cluster-administration-addons"><span>애드온 설치</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-extend-kubernetes-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-check">
<label for="m-ko-docs-concepts-extend-kubernetes-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-extend-kubernetes"><span>쿠버네티스 확장</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-extend-kubernetes-api-extension-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-api-extension-check">
<label for="m-ko-docs-concepts-extend-kubernetes-api-extension-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/api-extension/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-extend-kubernetes-api-extension"><span>쿠버네티스 API 확장하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-extend-kubernetes-api-extension-custom-resources-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-api-extension-custom-resources-check">
<label for="m-ko-docs-concepts-extend-kubernetes-api-extension-custom-resources-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/api-extension/custom-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-extend-kubernetes-api-extension-custom-resources"><span>커스텀 리소스</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-extend-kubernetes-api-extension-apiserver-aggregation-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-api-extension-apiserver-aggregation-check">
<label for="m-ko-docs-concepts-extend-kubernetes-api-extension-apiserver-aggregation-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-extend-kubernetes-api-extension-apiserver-aggregation"><span>쿠버네티스 API 애그리게이션 레이어(aggregation layer)</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-extend-kubernetes-operator-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-operator-check">
<label for="m-ko-docs-concepts-extend-kubernetes-operator-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/operator/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-extend-kubernetes-operator"><span>오퍼레이터(operator) 패턴</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-check">
<label for="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/compute-storage-net/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net"><span>컴퓨트, 스토리지 및 네트워킹 익스텐션</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-network-plugins-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-network-plugins-check">
<label for="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-network-plugins-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-network-plugins"><span>네트워크 플러그인</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-device-plugins-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-device-plugins-check">
<label for="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-device-plugins-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/compute-storage-net/device-plugins/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-extend-kubernetes-compute-storage-net-device-plugins"><span>장치 플러그인</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-concepts-extend-kubernetes-service-catalog-li">
<input type="checkbox" id="m-ko-docs-concepts-extend-kubernetes-service-catalog-check">
<label for="m-ko-docs-concepts-extend-kubernetes-service-catalog-check"><a href="https://kubernetes.io/ko/docs/concepts/extend-kubernetes/service-catalog/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-concepts-extend-kubernetes-service-catalog"><span>서비스 카탈로그</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-li">
<input type="checkbox" id="m-ko-docs-tasks-check">
<label for="m-ko-docs-tasks-check"><a href="https://kubernetes.io/ko/docs/tasks/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks"><span>태스크</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-tools-li">
<input type="checkbox" id="m-ko-docs-tasks-tools-check">
<label for="m-ko-docs-tasks-tools-check"><a href="https://kubernetes.io/ko/docs/tasks/tools/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-tools"><span>도구 설치</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-tools-install-kubectl-macos-li">
<input type="checkbox" id="m-ko-docs-tasks-tools-install-kubectl-macos-check">
<label for="m-ko-docs-tasks-tools-install-kubectl-macos-check"><a href="https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-macos/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-tools-install-kubectl-macos"><span>macOS에 kubectl 설치 및 설정</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-tools-install-kubectl-linux-li">
<input type="checkbox" id="m-ko-docs-tasks-tools-install-kubectl-linux-check">
<label for="m-ko-docs-tasks-tools-install-kubectl-linux-check"><a href="https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-tools-install-kubectl-linux"><span>리눅스에 kubectl 설치 및 설정</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-tools-install-kubectl-windows-li">
<input type="checkbox" id="m-ko-docs-tasks-tools-install-kubectl-windows-check">
<label for="m-ko-docs-tasks-tools-install-kubectl-windows-check"><a href="https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-windows/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-tools-install-kubectl-windows"><span>윈도우에 kubectl 설치 및 설정</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-tasks-debug-li">
<input type="checkbox" id="m-docs-tasks-debug-check">
<label for="m-docs-tasks-debug-check"><a href="https://kubernetes.io/docs/tasks/debug/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-tasks-debug"><span>Monitoring, Logging, and Debugging</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-tasks-debug-debug-application-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-check">
<label for="m-docs-tasks-debug-debug-application-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-tasks-debug-debug-application"><span>Troubleshooting Applications</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-debug-pods-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-debug-pods-check">
<label for="m-docs-tasks-debug-debug-application-debug-pods-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/debug-pods/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-debug-pods"><span>Debug Pods</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-debug-service-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-debug-service-check">
<label for="m-docs-tasks-debug-debug-application-debug-service-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-debug-service"><span>Debug Services</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-debug-statefulset-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-debug-statefulset-check">
<label for="m-docs-tasks-debug-debug-application-debug-statefulset-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/debug-statefulset/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-debug-statefulset"><span>Debug a StatefulSet</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-debug-init-containers-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-debug-init-containers-check">
<label for="m-docs-tasks-debug-debug-application-debug-init-containers-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/debug-init-containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-debug-init-containers"><span>Debug Init Containers</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-debug-running-pod-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-debug-running-pod-check">
<label for="m-docs-tasks-debug-debug-application-debug-running-pod-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-debug-running-pod"><span>Debug Running Pods</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-determine-reason-pod-failure-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-determine-reason-pod-failure-check">
<label for="m-docs-tasks-debug-debug-application-determine-reason-pod-failure-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/determine-reason-pod-failure/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-determine-reason-pod-failure"><span>Determine the Reason for Pod Failure</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-application-get-shell-running-container-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-application-get-shell-running-container-check">
<label for="m-docs-tasks-debug-debug-application-get-shell-running-container-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-application-get-shell-running-container"><span>Get a Shell to a Running Container</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-tasks-debug-debug-cluster-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-check">
<label for="m-docs-tasks-debug-debug-cluster-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-tasks-debug-debug-cluster"><span>Troubleshooting Clusters</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-cluster-resource-metrics-pipeline-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-resource-metrics-pipeline-check">
<label for="m-docs-tasks-debug-debug-cluster-resource-metrics-pipeline-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-cluster-resource-metrics-pipeline"><span>Resource metrics pipeline</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-cluster-resource-usage-monitoring-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-resource-usage-monitoring-check">
<label for="m-docs-tasks-debug-debug-cluster-resource-usage-monitoring-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-cluster-resource-usage-monitoring"><span>Tools for Monitoring Resources</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-cluster-monitor-node-health-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-monitor-node-health-check">
<label for="m-docs-tasks-debug-debug-cluster-monitor-node-health-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/monitor-node-health/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-cluster-monitor-node-health"><span>Monitor Node Health</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-cluster-crictl-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-crictl-check">
<label for="m-docs-tasks-debug-debug-cluster-crictl-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-cluster-crictl"><span>Debugging Kubernetes nodes with crictl</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-cluster-audit-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-audit-check">
<label for="m-docs-tasks-debug-debug-cluster-audit-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-cluster-audit"><span>Auditing</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-debug-debug-cluster-local-debugging-li">
<input type="checkbox" id="m-docs-tasks-debug-debug-cluster-local-debugging-check">
<label for="m-docs-tasks-debug-debug-cluster-local-debugging-check"><a href="https://kubernetes.io/docs/tasks/debug/debug-cluster/local-debugging/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-debug-debug-cluster-local-debugging"><span>Developing and debugging services locally using telepresence</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-administer-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-check">
<label for="m-ko-docs-tasks-administer-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-administer-cluster"><span>클러스터 운영</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-administer-cluster-kubeadm-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-kubeadm-check">
<label for="m-ko-docs-tasks-administer-cluster-kubeadm-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-administer-cluster-kubeadm"><span>kubeadm으로 관리하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-kubeadm-configure-cgroup-driver-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-kubeadm-configure-cgroup-driver-check">
<label for="m-docs-tasks-administer-cluster-kubeadm-configure-cgroup-driver-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/configure-cgroup-driver/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-kubeadm-configure-cgroup-driver"><span>Configuring a cgroup driver</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-certs-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-certs-check">
<label for="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-certs-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-certs"><span>kubeadm을 사용한 인증서 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-kubeadm-kubeadm-reconfigure-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-kubeadm-kubeadm-reconfigure-check">
<label for="m-docs-tasks-administer-cluster-kubeadm-kubeadm-reconfigure-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-reconfigure/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-kubeadm-kubeadm-reconfigure"><span>Reconfiguring a kubeadm cluster</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-upgrade-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-upgrade-check">
<label for="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-upgrade-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-kubeadm-kubeadm-upgrade"><span>kubeadm 클러스터 업그레이드</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-kubeadm-adding-windows-nodes-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-kubeadm-adding-windows-nodes-check">
<label for="m-ko-docs-tasks-administer-cluster-kubeadm-adding-windows-nodes-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/adding-windows-nodes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-kubeadm-adding-windows-nodes"><span>윈도우 노드 추가</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-kubeadm-upgrading-windows-nodes-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-kubeadm-upgrading-windows-nodes-check">
<label for="m-ko-docs-tasks-administer-cluster-kubeadm-upgrading-windows-nodes-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/upgrading-windows-nodes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-kubeadm-upgrading-windows-nodes"><span>윈도우 노드 업그레이드</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-tasks-administer-cluster-migrating-from-dockershim"><span>Migrating from dockershim</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-change-runtime-containerd-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-change-runtime-containerd-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-change-runtime-containerd-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/change-runtime-containerd/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-change-runtime-containerd"><span>Changing the Container Runtime on a Node from Docker Engine to containerd</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrate-dockershim-dockerd-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrate-dockershim-dockerd-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrate-dockershim-dockerd-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/migrate-dockershim-dockerd/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrate-dockershim-dockerd"><span>Migrate Docker Engine nodes from dockershim to cri-dockerd</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-find-out-runtime-you-use-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-find-out-runtime-you-use-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-find-out-runtime-you-use-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/find-out-runtime-you-use/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-find-out-runtime-you-use"><span>Find Out What Container Runtime is Used on a Node</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-troubleshooting-cni-plugin-related-errors-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-troubleshooting-cni-plugin-related-errors-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-troubleshooting-cni-plugin-related-errors-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/troubleshooting-cni-plugin-related-errors/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-troubleshooting-cni-plugin-related-errors"><span>Troubleshooting CNI plugin-related errors</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-check-if-dockershim-removal-affects-you-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-check-if-dockershim-removal-affects-you-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-check-if-dockershim-removal-affects-you-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/check-if-dockershim-removal-affects-you/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-check-if-dockershim-removal-affects-you"><span>Check whether dockershim removal affects you</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrating-telemetry-and-security-agents-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrating-telemetry-and-security-agents-check">
<label for="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrating-telemetry-and-security-agents-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/migrating-telemetry-and-security-agents/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-migrating-from-dockershim-migrating-telemetry-and-security-agents"><span>Migrating telemetry and security agents from dockershim</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-administer-cluster-manage-resources"><span>메모리, CPU 와 API 리소스 관리</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-memory-default-namespace-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-memory-default-namespace-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-memory-default-namespace-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-manage-resources-memory-default-namespace"><span>네임스페이스에 대한 기본 메모리 요청량과 상한 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-default-namespace-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-default-namespace-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-default-namespace-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-default-namespace"><span>네임스페이스에 대한 기본 CPU 요청량과 상한 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-memory-constraint-namespace-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-memory-constraint-namespace-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-memory-constraint-namespace-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/memory-constraint-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-manage-resources-memory-constraint-namespace"><span>네임스페이스에 대한 메모리의 최소 및 최대 제약 조건 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-constraint-namespace-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-constraint-namespace-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-constraint-namespace-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/cpu-constraint-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-manage-resources-cpu-constraint-namespace"><span>네임스페이스에 대한 CPU의 최소 및 최대 제약 조건 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-quota-memory-cpu-namespace-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-quota-memory-cpu-namespace-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-quota-memory-cpu-namespace-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-manage-resources-quota-memory-cpu-namespace"><span>네임스페이스에 대한 메모리 및 CPU 쿼터 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-manage-resources-quota-pod-namespace-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-manage-resources-quota-pod-namespace-check">
<label for="m-ko-docs-tasks-administer-cluster-manage-resources-quota-pod-namespace-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/quota-pod-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-manage-resources-quota-pod-namespace"><span>네임스페이스에 대한 파드 쿼터 구성</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-certificates-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-certificates-check">
<label for="m-ko-docs-tasks-administer-cluster-certificates-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/certificates/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-certificates"><span>인증서</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-check">
<label for="m-ko-docs-tasks-administer-cluster-network-policy-provider-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/network-policy-provider/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-administer-cluster-network-policy-provider"><span>네트워크 폴리시 제공자(Network Policy Provider) 설치</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-network-policy-provider-antrea-network-policy-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-network-policy-provider-antrea-network-policy-check">
<label for="m-docs-tasks-administer-cluster-network-policy-provider-antrea-network-policy-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/antrea-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-network-policy-provider-antrea-network-policy"><span>Use Antrea for NetworkPolicy</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-calico-network-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-calico-network-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-network-policy-provider-calico-network-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/network-policy-provider/calico-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-calico-network-policy"><span>네트워크 폴리시로 캘리코(Calico) 사용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-cilium-network-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-cilium-network-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-network-policy-provider-cilium-network-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/network-policy-provider/cilium-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-cilium-network-policy"><span>네트워크 폴리시로 실리움(Cilium) 사용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-kube-router-network-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-kube-router-network-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-network-policy-provider-kube-router-network-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/network-policy-provider/kube-router-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-kube-router-network-policy"><span>네트워크 폴리시로 큐브 라우터(Kube-router) 사용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-romana-network-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-romana-network-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-network-policy-provider-romana-network-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/network-policy-provider/romana-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-romana-network-policy"><span>네트워크 폴리시로 로마나(Romana)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-weave-network-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-weave-network-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-network-policy-provider-weave-network-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/network-policy-provider/weave-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-network-policy-provider-weave-network-policy"><span>네트워크 폴리시로 위브넷(Weave Net) 사용하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-dns-horizontal-autoscaling-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-dns-horizontal-autoscaling-check">
<label for="m-docs-tasks-administer-cluster-dns-horizontal-autoscaling-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-dns-horizontal-autoscaling"><span>Autoscale the DNS Service in a Cluster</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-running-cloud-controller-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-running-cloud-controller-check">
<label for="m-docs-tasks-administer-cluster-running-cloud-controller-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-running-cloud-controller"><span>Cloud Controller Manager Administration</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-quota-api-object-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-quota-api-object-check">
<label for="m-docs-tasks-administer-cluster-quota-api-object-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/quota-api-object/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-quota-api-object"><span>Configure Quotas for API Objects</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-cpu-management-policies-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-cpu-management-policies-check">
<label for="m-docs-tasks-administer-cluster-cpu-management-policies-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/cpu-management-policies/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-cpu-management-policies"><span>Control CPU Management Policies on the Node</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-topology-manager-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-topology-manager-check">
<label for="m-docs-tasks-administer-cluster-topology-manager-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/topology-manager/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-topology-manager"><span>Control Topology Management Policies on a node</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-dns-debugging-resolution-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-dns-debugging-resolution-check">
<label for="m-docs-tasks-administer-cluster-dns-debugging-resolution-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-dns-debugging-resolution"><span>Debugging DNS Resolution</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-developing-cloud-controller-manager-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-developing-cloud-controller-manager-check">
<label for="m-docs-tasks-administer-cluster-developing-cloud-controller-manager-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/developing-cloud-controller-manager/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-developing-cloud-controller-manager"><span>Developing Cloud Controller Manager</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-dns-custom-nameservers-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-dns-custom-nameservers-check">
<label for="m-ko-docs-tasks-administer-cluster-dns-custom-nameservers-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/dns-custom-nameservers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-dns-custom-nameservers"><span>DNS 서비스 사용자 정의하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-enabling-service-topology-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-enabling-service-topology-check">
<label for="m-docs-tasks-administer-cluster-enabling-service-topology-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/enabling-service-topology/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-enabling-service-topology"><span>Enabling Service Topology</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-encrypt-data-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-encrypt-data-check">
<label for="m-docs-tasks-administer-cluster-encrypt-data-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-encrypt-data"><span>Encrypting Secret Data at Rest</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-ip-masq-agent-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-ip-masq-agent-check">
<label for="m-docs-tasks-administer-cluster-ip-masq-agent-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/ip-masq-agent/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-ip-masq-agent"><span>IP Masquerade Agent User Guide</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-limit-storage-consumption-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-limit-storage-consumption-check">
<label for="m-docs-tasks-administer-cluster-limit-storage-consumption-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/limit-storage-consumption/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-limit-storage-consumption"><span>Limit Storage Consumption</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-controller-manager-leader-migration-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-controller-manager-leader-migration-check">
<label for="m-docs-tasks-administer-cluster-controller-manager-leader-migration-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/controller-manager-leader-migration/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-controller-manager-leader-migration"><span>Migrate Replicated Control Plane To Use Cloud Controller Manager</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-namespaces-walkthrough-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-namespaces-walkthrough-check">
<label for="m-docs-tasks-administer-cluster-namespaces-walkthrough-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-namespaces-walkthrough"><span>Namespaces Walkthrough</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-configure-upgrade-etcd-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-configure-upgrade-etcd-check">
<label for="m-docs-tasks-administer-cluster-configure-upgrade-etcd-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-configure-upgrade-etcd"><span>Operating etcd clusters for Kubernetes</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-reconfigure-kubelet-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-reconfigure-kubelet-check">
<label for="m-docs-tasks-administer-cluster-reconfigure-kubelet-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/reconfigure-kubelet/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-reconfigure-kubelet"><span>Reconfigure a Node's Kubelet in a Live Cluster</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-reserve-compute-resources-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-reserve-compute-resources-check">
<label for="m-docs-tasks-administer-cluster-reserve-compute-resources-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/reserve-compute-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-reserve-compute-resources"><span>Reserve Compute Resources for System Daemons</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-kubelet-in-userns-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-kubelet-in-userns-check">
<label for="m-docs-tasks-administer-cluster-kubelet-in-userns-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/kubelet-in-userns/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-kubelet-in-userns"><span>Running Kubernetes Node Components as a Non-root User</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-safely-drain-node-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-safely-drain-node-check">
<label for="m-docs-tasks-administer-cluster-safely-drain-node-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-safely-drain-node"><span>Safely Drain a Node</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-securing-a-cluster-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-securing-a-cluster-check">
<label for="m-docs-tasks-administer-cluster-securing-a-cluster-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-securing-a-cluster"><span>Securing a Cluster</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-kubelet-config-file-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-kubelet-config-file-check">
<label for="m-docs-tasks-administer-cluster-kubelet-config-file-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/kubelet-config-file/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-kubelet-config-file"><span>Set Kubelet parameters via a config file</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-namespaces-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-namespaces-check">
<label for="m-docs-tasks-administer-cluster-namespaces-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/namespaces/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-namespaces"><span>Share a Cluster with Namespaces</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-cluster-upgrade-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-cluster-upgrade-check">
<label for="m-docs-tasks-administer-cluster-cluster-upgrade-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/cluster-upgrade/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-cluster-upgrade"><span>Upgrade A Cluster</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-use-cascading-deletion-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-use-cascading-deletion-check">
<label for="m-docs-tasks-administer-cluster-use-cascading-deletion-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/use-cascading-deletion/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-use-cascading-deletion"><span>Use Cascading Deletion in a Cluster</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-kms-provider-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-kms-provider-check">
<label for="m-docs-tasks-administer-cluster-kms-provider-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-kms-provider"><span>Using a KMS provider for data encryption</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-nodelocaldns-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-nodelocaldns-check">
<label for="m-docs-tasks-administer-cluster-nodelocaldns-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/nodelocaldns/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-nodelocaldns"><span>Using NodeLocal DNSCache in Kubernetes clusters</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-memory-manager-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-memory-manager-check">
<label for="m-docs-tasks-administer-cluster-memory-manager-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/memory-manager/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-memory-manager"><span>Utilizing the NUMA-aware Memory Manager</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-administer-cluster-verify-signed-images-li">
<input type="checkbox" id="m-docs-tasks-administer-cluster-verify-signed-images-check">
<label for="m-docs-tasks-administer-cluster-verify-signed-images-check"><a href="https://kubernetes.io/docs/tasks/administer-cluster/verify-signed-images/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-administer-cluster-verify-signed-images"><span>Verify Signed Container Images</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-change-default-storage-class-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-change-default-storage-class-check">
<label for="m-ko-docs-tasks-administer-cluster-change-default-storage-class-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/change-default-storage-class/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-change-default-storage-class"><span>기본 스토리지클래스(StorageClass) 변경하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-declare-network-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-declare-network-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-declare-network-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/declare-network-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-declare-network-policy"><span>네트워크 폴리시(Network Policy) 선언하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-extended-resource-node-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-extended-resource-node-check">
<label for="m-ko-docs-tasks-administer-cluster-extended-resource-node-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/extended-resource-node/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-extended-resource-node"><span>노드에 대한 확장 리소스 알리기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-coredns-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-coredns-check">
<label for="m-ko-docs-tasks-administer-cluster-coredns-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/coredns/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-coredns"><span>서비스 디스커버리를 위해 CoreDNS 사용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-guaranteed-scheduling-critical-addon-pods-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-guaranteed-scheduling-critical-addon-pods-check">
<label for="m-ko-docs-tasks-administer-cluster-guaranteed-scheduling-critical-addon-pods-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/guaranteed-scheduling-critical-addon-pods/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-guaranteed-scheduling-critical-addon-pods"><span>중요한 애드온 파드 스케줄링 보장하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-enable-disable-api-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-enable-disable-api-check">
<label for="m-ko-docs-tasks-administer-cluster-enable-disable-api-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/enable-disable-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-enable-disable-api"><span>쿠버네티스 API 활성화 혹은 비활성화하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-access-cluster-api-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-access-cluster-api-check">
<label for="m-ko-docs-tasks-administer-cluster-access-cluster-api-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/access-cluster-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-access-cluster-api"><span>쿠버네티스 API를 사용하여 클러스터에 접근하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-sysctl-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-sysctl-cluster-check">
<label for="m-ko-docs-tasks-administer-cluster-sysctl-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/sysctl-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-sysctl-cluster"><span>쿠버네티스 클러스터에서 sysctl 사용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-access-cluster-services-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-access-cluster-services-check">
<label for="m-ko-docs-tasks-administer-cluster-access-cluster-services-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/access-cluster-services/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-access-cluster-services"><span>클러스터에서 실행되는 서비스에 접근</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-administer-cluster-change-pv-reclaim-policy-li">
<input type="checkbox" id="m-ko-docs-tasks-administer-cluster-change-pv-reclaim-policy-check">
<label for="m-ko-docs-tasks-administer-cluster-change-pv-reclaim-policy-check"><a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/change-pv-reclaim-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-administer-cluster-change-pv-reclaim-policy"><span>퍼시스턴트볼륨 반환 정책 변경하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-configure-pod-container-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-check">
<label for="m-ko-docs-tasks-configure-pod-container-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-configure-pod-container"><span>파드와 컨테이너 설정</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-assign-memory-resource-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-assign-memory-resource-check">
<label for="m-ko-docs-tasks-configure-pod-container-assign-memory-resource-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/assign-memory-resource/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-assign-memory-resource"><span>컨테이너 및 파드 메모리 리소스 할당</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-assign-cpu-resource-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-assign-cpu-resource-check">
<label for="m-docs-tasks-configure-pod-container-assign-cpu-resource-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-assign-cpu-resource"><span>Assign CPU Resources to Containers and Pods</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-create-hostprocess-pod-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-create-hostprocess-pod-check">
<label for="m-docs-tasks-configure-pod-container-create-hostprocess-pod-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/create-hostprocess-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-create-hostprocess-pod"><span>Create a Windows HostProcess Pod</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-configure-runasusername-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-configure-runasusername-check">
<label for="m-ko-docs-tasks-configure-pod-container-configure-runasusername-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/configure-runasusername/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-configure-runasusername"><span>윈도우 파드 및 컨테이너에서 RunAsUserName 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-configure-gmsa-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-configure-gmsa-check">
<label for="m-ko-docs-tasks-configure-pod-container-configure-gmsa-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/configure-gmsa/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-configure-gmsa"><span>윈도우 파드와 컨테이너용 GMSA 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-quality-service-pod-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-quality-service-pod-check">
<label for="m-ko-docs-tasks-configure-pod-container-quality-service-pod-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/quality-service-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-quality-service-pod"><span>파드에 대한 서비스 품질(QoS) 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-extended-resource-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-extended-resource-check">
<label for="m-docs-tasks-configure-pod-container-extended-resource-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/extended-resource/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-extended-resource"><span>Assign Extended Resources to a Container</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-configure-volume-storage-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-configure-volume-storage-check">
<label for="m-ko-docs-tasks-configure-pod-container-configure-volume-storage-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/configure-volume-storage/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-configure-volume-storage"><span>스토리지의 볼륨을 사용하는 파드 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-configure-persistent-volume-storage-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-configure-persistent-volume-storage-check">
<label for="m-ko-docs-tasks-configure-pod-container-configure-persistent-volume-storage-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/configure-persistent-volume-storage/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-configure-persistent-volume-storage"><span>스토리지로 퍼시스턴트볼륨(PersistentVolume)을 사용하도록 파드 설정하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-configure-projected-volume-storage-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-configure-projected-volume-storage-check">
<label for="m-docs-tasks-configure-pod-container-configure-projected-volume-storage-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-projected-volume-storage/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-configure-projected-volume-storage"><span>Configure a Pod to Use a Projected Volume for Storage</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-security-context-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-security-context-check">
<label for="m-docs-tasks-configure-pod-container-security-context-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/security-context/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-security-context"><span>Configure a Security Context for a Pod or Container</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-configure-service-account-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-configure-service-account-check">
<label for="m-docs-tasks-configure-pod-container-configure-service-account-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-configure-service-account"><span>Configure Service Accounts for Pods</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-pull-image-private-registry-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-pull-image-private-registry-check">
<label for="m-ko-docs-tasks-configure-pod-container-pull-image-private-registry-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/pull-image-private-registry/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-pull-image-private-registry"><span>프라이빗 레지스트리에서 이미지 받아오기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-configure-liveness-readiness-startup-probes-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-configure-liveness-readiness-startup-probes-check">
<label for="m-docs-tasks-configure-pod-container-configure-liveness-readiness-startup-probes-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-configure-liveness-readiness-startup-probes"><span>Configure Liveness, Readiness and Startup Probes</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-using-node-affinity-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-using-node-affinity-check">
<label for="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-using-node-affinity-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-using-node-affinity"><span>노드 어피니티를 사용해 노드에 파드 할당</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-check">
<label for="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/assign-pods-nodes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-assign-pods-nodes"><span>노드에 파드 할당</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-configure-pod-initialization-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-configure-pod-initialization-check">
<label for="m-ko-docs-tasks-configure-pod-container-configure-pod-initialization-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/configure-pod-initialization/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-configure-pod-initialization"><span>초기화 컨테이너에 대한 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-attach-handler-lifecycle-event-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-attach-handler-lifecycle-event-check">
<label for="m-docs-tasks-configure-pod-container-attach-handler-lifecycle-event-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-attach-handler-lifecycle-event"><span>Attach Handlers to Container Lifecycle Events</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-configure-pod-configmap-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-configure-pod-configmap-check">
<label for="m-docs-tasks-configure-pod-container-configure-pod-configmap-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-configure-pod-configmap"><span>Configure a Pod to Use a ConfigMap</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-share-process-namespace-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-share-process-namespace-check">
<label for="m-docs-tasks-configure-pod-container-share-process-namespace-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/share-process-namespace/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-share-process-namespace"><span>Share Process Namespace between Containers in a Pod</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configure-pod-container-static-pod-li">
<input type="checkbox" id="m-ko-docs-tasks-configure-pod-container-static-pod-check">
<label for="m-ko-docs-tasks-configure-pod-container-static-pod-check"><a href="https://kubernetes.io/ko/docs/tasks/configure-pod-container/static-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configure-pod-container-static-pod"><span>스태틱(static) 파드 생성하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-translate-compose-kubernetes-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-translate-compose-kubernetes-check">
<label for="m-docs-tasks-configure-pod-container-translate-compose-kubernetes-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/translate-compose-kubernetes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-translate-compose-kubernetes"><span>Translate a Docker Compose File to Kubernetes Resources</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-enforce-standards-admission-controller-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-enforce-standards-admission-controller-check">
<label for="m-docs-tasks-configure-pod-container-enforce-standards-admission-controller-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/enforce-standards-admission-controller/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-enforce-standards-admission-controller"><span>Enforce Pod Security Standards by Configuring the Built-in Admission Controller</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-enforce-standards-namespace-labels-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-enforce-standards-namespace-labels-check">
<label for="m-docs-tasks-configure-pod-container-enforce-standards-namespace-labels-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/enforce-standards-namespace-labels/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-enforce-standards-namespace-labels"><span>Enforce Pod Security Standards with Namespace Labels</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-configure-pod-container-migrate-from-psp-li">
<input type="checkbox" id="m-docs-tasks-configure-pod-container-migrate-from-psp-check">
<label for="m-docs-tasks-configure-pod-container-migrate-from-psp-check"><a href="https://kubernetes.io/docs/tasks/configure-pod-container/migrate-from-psp/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-configure-pod-container-migrate-from-psp"><span>Migrate from PodSecurityPolicy to the Built-In PodSecurity Admission Controller</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-manage-kubernetes-objects-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-kubernetes-objects-check">
<label for="m-ko-docs-tasks-manage-kubernetes-objects-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-manage-kubernetes-objects"><span>쿠버네티스 오브젝트 관리</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-kubernetes-objects-declarative-config-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-kubernetes-objects-declarative-config-check">
<label for="m-ko-docs-tasks-manage-kubernetes-objects-declarative-config-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/declarative-config/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-kubernetes-objects-declarative-config"><span>구성 파일을 이용한 쿠버네티스 오브젝트의 선언형 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-kubernetes-objects-kustomization-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-kubernetes-objects-kustomization-check">
<label for="m-ko-docs-tasks-manage-kubernetes-objects-kustomization-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/kustomization/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-kubernetes-objects-kustomization"><span>Kustomize를 이용한 쿠버네티스 오브젝트의 선언형 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-kubernetes-objects-imperative-command-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-kubernetes-objects-imperative-command-check">
<label for="m-ko-docs-tasks-manage-kubernetes-objects-imperative-command-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/imperative-command/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-kubernetes-objects-imperative-command"><span>명령형 커맨드를 이용한 쿠버네티스 오브젝트 관리하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-kubernetes-objects-imperative-config-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-kubernetes-objects-imperative-config-check">
<label for="m-ko-docs-tasks-manage-kubernetes-objects-imperative-config-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/imperative-config/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-kubernetes-objects-imperative-config"><span>구성파일을 이용한 명령형 쿠버네티스 오브젝트 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-manage-kubernetes-objects-update-api-object-kubectl-patch-li">
<input type="checkbox" id="m-docs-tasks-manage-kubernetes-objects-update-api-object-kubectl-patch-check">
<label for="m-docs-tasks-manage-kubernetes-objects-update-api-object-kubectl-patch-check"><a href="https://kubernetes.io/docs/tasks/manage-kubernetes-objects/update-api-object-kubectl-patch/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-manage-kubernetes-objects-update-api-object-kubectl-patch"><span>Update API Objects in Place Using kubectl patch</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-configmap-secret-li">
<input type="checkbox" id="m-ko-docs-tasks-configmap-secret-check">
<label for="m-ko-docs-tasks-configmap-secret-check"><a href="https://kubernetes.io/ko/docs/tasks/configmap-secret/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-configmap-secret"><span>시크릿(Secret) 관리</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-kubectl-li">
<input type="checkbox" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-kubectl-check">
<label for="m-ko-docs-tasks-configmap-secret-managing-secret-using-kubectl-check"><a href="https://kubernetes.io/ko/docs/tasks/configmap-secret/managing-secret-using-kubectl/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-kubectl"><span>kubectl을 사용한 시크릿 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-config-file-li">
<input type="checkbox" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-config-file-check">
<label for="m-ko-docs-tasks-configmap-secret-managing-secret-using-config-file-check"><a href="https://kubernetes.io/ko/docs/tasks/configmap-secret/managing-secret-using-config-file/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-config-file"><span>환경 설정 파일을 사용하여 시크릿을 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-kustomize-li">
<input type="checkbox" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-kustomize-check">
<label for="m-ko-docs-tasks-configmap-secret-managing-secret-using-kustomize-check"><a href="https://kubernetes.io/ko/docs/tasks/configmap-secret/managing-secret-using-kustomize/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-configmap-secret-managing-secret-using-kustomize"><span>kustomize를 사용하여 시크릿 관리</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-inject-data-application-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-check">
<label for="m-ko-docs-tasks-inject-data-application-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-inject-data-application"><span>애플리케이션에 데이터 주입하기</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-inject-data-application-define-command-argument-container-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-define-command-argument-container-check">
<label for="m-ko-docs-tasks-inject-data-application-define-command-argument-container-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/define-command-argument-container/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-inject-data-application-define-command-argument-container"><span>컨테이너를 위한 커맨드와 인자 정의하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-inject-data-application-define-interdependent-environment-variables-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-define-interdependent-environment-variables-check">
<label for="m-ko-docs-tasks-inject-data-application-define-interdependent-environment-variables-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/define-interdependent-environment-variables/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-inject-data-application-define-interdependent-environment-variables"><span>종속 환경 변수 정의하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-inject-data-application-define-environment-variable-container-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-define-environment-variable-container-check">
<label for="m-ko-docs-tasks-inject-data-application-define-environment-variable-container-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/define-environment-variable-container/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-inject-data-application-define-environment-variable-container"><span>컨테이너를 위한 환경 변수 정의하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-inject-data-application-environment-variable-expose-pod-information-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-environment-variable-expose-pod-information-check">
<label for="m-ko-docs-tasks-inject-data-application-environment-variable-expose-pod-information-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/environment-variable-expose-pod-information/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-inject-data-application-environment-variable-expose-pod-information"><span>환경 변수로 컨테이너에 파드 정보 노출하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-inject-data-application-downward-api-volume-expose-pod-information-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-downward-api-volume-expose-pod-information-check">
<label for="m-ko-docs-tasks-inject-data-application-downward-api-volume-expose-pod-information-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/downward-api-volume-expose-pod-information/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-inject-data-application-downward-api-volume-expose-pod-information"><span>파일로 컨테이너에 파드 정보 노출하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-inject-data-application-distribute-credentials-secure-li">
<input type="checkbox" id="m-ko-docs-tasks-inject-data-application-distribute-credentials-secure-check">
<label for="m-ko-docs-tasks-inject-data-application-distribute-credentials-secure-check"><a href="https://kubernetes.io/ko/docs/tasks/inject-data-application/distribute-credentials-secure/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-inject-data-application-distribute-credentials-secure"><span>시크릿(Secret)을 사용하여 안전하게 자격증명 배포하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-run-application-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-check">
<label for="m-ko-docs-tasks-run-application-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-run-application"><span>애플리케이션 실행</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-run-stateless-application-deployment-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-run-stateless-application-deployment-check">
<label for="m-ko-docs-tasks-run-application-run-stateless-application-deployment-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/run-stateless-application-deployment/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-run-stateless-application-deployment"><span>디플로이먼트(Deployment)로 스테이트리스 애플리케이션 실행하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-run-single-instance-stateful-application-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-run-single-instance-stateful-application-check">
<label for="m-ko-docs-tasks-run-application-run-single-instance-stateful-application-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/run-single-instance-stateful-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-run-single-instance-stateful-application"><span>단일 인스턴스 스테이트풀 애플리케이션 실행하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-run-application-run-replicated-stateful-application-li">
<input type="checkbox" id="m-docs-tasks-run-application-run-replicated-stateful-application-check">
<label for="m-docs-tasks-run-application-run-replicated-stateful-application-check"><a href="https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-run-application-run-replicated-stateful-application"><span>Run a Replicated Stateful Application</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-scale-stateful-set-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-scale-stateful-set-check">
<label for="m-ko-docs-tasks-run-application-scale-stateful-set-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/scale-stateful-set/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-scale-stateful-set"><span>스테이트풀셋(StatefulSet) 확장하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-delete-stateful-set-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-delete-stateful-set-check">
<label for="m-ko-docs-tasks-run-application-delete-stateful-set-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/delete-stateful-set/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-delete-stateful-set"><span>스테이트풀셋(StatefulSet) 삭제하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-force-delete-stateful-set-pod-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-force-delete-stateful-set-pod-check">
<label for="m-ko-docs-tasks-run-application-force-delete-stateful-set-pod-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/force-delete-stateful-set-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-force-delete-stateful-set-pod"><span>스테이트풀셋(StatefulSet) 파드 강제 삭제하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-check">
<label for="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-horizontal-pod-autoscale"><span>Horizontal Pod Autoscaling</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-walkthrough-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-walkthrough-check">
<label for="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-walkthrough-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-horizontal-pod-autoscale-walkthrough"><span>HorizontalPodAutoscaler 연습</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-run-application-configure-pdb-li">
<input type="checkbox" id="m-docs-tasks-run-application-configure-pdb-check">
<label for="m-docs-tasks-run-application-configure-pdb-check"><a href="https://kubernetes.io/docs/tasks/run-application/configure-pdb/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-run-application-configure-pdb"><span>Specifying a Disruption Budget for your Application</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-run-application-access-api-from-pod-li">
<input type="checkbox" id="m-ko-docs-tasks-run-application-access-api-from-pod-check">
<label for="m-ko-docs-tasks-run-application-access-api-from-pod-check"><a href="https://kubernetes.io/ko/docs/tasks/run-application/access-api-from-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-run-application-access-api-from-pod"><span>파드 내에서 쿠버네티스 API에 접근</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-job-li">
<input type="checkbox" id="m-ko-docs-tasks-job-check">
<label for="m-ko-docs-tasks-job-check"><a href="https://kubernetes.io/ko/docs/tasks/job/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-job"><span>잡(Job) 실행</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-job-automated-tasks-with-cron-jobs-li">
<input type="checkbox" id="m-ko-docs-tasks-job-automated-tasks-with-cron-jobs-check">
<label for="m-ko-docs-tasks-job-automated-tasks-with-cron-jobs-check"><a href="https://kubernetes.io/ko/docs/tasks/job/automated-tasks-with-cron-jobs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-job-automated-tasks-with-cron-jobs"><span>크론잡(CronJob)으로 자동화된 작업 실행</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-job-coarse-parallel-processing-work-queue-li">
<input type="checkbox" id="m-ko-docs-tasks-job-coarse-parallel-processing-work-queue-check">
<label for="m-ko-docs-tasks-job-coarse-parallel-processing-work-queue-check"><a href="https://kubernetes.io/ko/docs/tasks/job/coarse-parallel-processing-work-queue/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-job-coarse-parallel-processing-work-queue"><span>작업 대기열을 사용한 거친 병렬 처리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-job-indexed-parallel-processing-static-li">
<input type="checkbox" id="m-docs-tasks-job-indexed-parallel-processing-static-check">
<label for="m-docs-tasks-job-indexed-parallel-processing-static-check"><a href="https://kubernetes.io/docs/tasks/job/indexed-parallel-processing-static/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-job-indexed-parallel-processing-static"><span>Indexed Job for Parallel Processing with Static Work Assignment</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-job-fine-parallel-processing-work-queue-li">
<input type="checkbox" id="m-ko-docs-tasks-job-fine-parallel-processing-work-queue-check">
<label for="m-ko-docs-tasks-job-fine-parallel-processing-work-queue-check"><a href="https://kubernetes.io/ko/docs/tasks/job/fine-parallel-processing-work-queue/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-job-fine-parallel-processing-work-queue"><span>작업 대기열을 사용한 정밀 병렬 처리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-job-parallel-processing-expansion-li">
<input type="checkbox" id="m-ko-docs-tasks-job-parallel-processing-expansion-check">
<label for="m-ko-docs-tasks-job-parallel-processing-expansion-check"><a href="https://kubernetes.io/ko/docs/tasks/job/parallel-processing-expansion/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-job-parallel-processing-expansion"><span>확장을 사용한 병렬 처리</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-access-application-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-check">
<label for="m-ko-docs-tasks-access-application-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-access-application-cluster"><span>클러스터 내 어플리케이션 접근</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-web-ui-dashboard-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-web-ui-dashboard-check">
<label for="m-ko-docs-tasks-access-application-cluster-web-ui-dashboard-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-web-ui-dashboard"><span>쿠버네티스 대시보드를 배포하고 접속하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-access-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-access-cluster-check">
<label for="m-ko-docs-tasks-access-application-cluster-access-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/access-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-access-cluster"><span>클러스터 접근</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-configure-access-multiple-clusters-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-configure-access-multiple-clusters-check">
<label for="m-ko-docs-tasks-access-application-cluster-configure-access-multiple-clusters-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/configure-access-multiple-clusters/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-configure-access-multiple-clusters"><span>다중 클러스터 접근 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-port-forward-access-application-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-port-forward-access-application-cluster-check">
<label for="m-ko-docs-tasks-access-application-cluster-port-forward-access-application-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/port-forward-access-application-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-port-forward-access-application-cluster"><span>포트 포워딩을 사용해서 클러스터 내 애플리케이션에 접근하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-service-access-application-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-service-access-application-cluster-check">
<label for="m-ko-docs-tasks-access-application-cluster-service-access-application-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/service-access-application-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-service-access-application-cluster"><span>클러스터 내 애플리케이션에 접근하기 위해 서비스 사용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-connecting-frontend-backend-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-connecting-frontend-backend-check">
<label for="m-ko-docs-tasks-access-application-cluster-connecting-frontend-backend-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/connecting-frontend-backend/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-connecting-frontend-backend"><span>서비스를 사용하여 프론트엔드를 백엔드에 연결</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-access-application-cluster-create-external-load-balancer-li">
<input type="checkbox" id="m-docs-tasks-access-application-cluster-create-external-load-balancer-check">
<label for="m-docs-tasks-access-application-cluster-create-external-load-balancer-check"><a href="https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-access-application-cluster-create-external-load-balancer"><span>Create an External Load Balancer</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-ingress-minikube-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-ingress-minikube-check">
<label for="m-ko-docs-tasks-access-application-cluster-ingress-minikube-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/ingress-minikube/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-ingress-minikube"><span>NGINX 인그레스(Ingress) 컨트롤러로 Minikube에서 인그레스 설정하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-list-all-running-container-images-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-list-all-running-container-images-check">
<label for="m-ko-docs-tasks-access-application-cluster-list-all-running-container-images-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/list-all-running-container-images/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-list-all-running-container-images"><span>클러스터 내 모든 컨테이너 이미지 목록 보기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-communicate-containers-same-pod-shared-volume-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-communicate-containers-same-pod-shared-volume-check">
<label for="m-ko-docs-tasks-access-application-cluster-communicate-containers-same-pod-shared-volume-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-communicate-containers-same-pod-shared-volume"><span>공유 볼륨을 이용하여 동일한 파드의 컨테이너 간에 통신하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-access-application-cluster-configure-dns-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-access-application-cluster-configure-dns-cluster-check">
<label for="m-ko-docs-tasks-access-application-cluster-configure-dns-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/configure-dns-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-access-application-cluster-configure-dns-cluster"><span>클러스터의 DNS 구성하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-access-application-cluster-access-cluster-services-li">
<input type="checkbox" id="m-docs-tasks-access-application-cluster-access-cluster-services-check">
<label for="m-docs-tasks-access-application-cluster-access-cluster-services-check"><a href="https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster-services/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-access-application-cluster-access-cluster-services"><span>Access Services Running on Clusters</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-debug-application-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-check">
<label for="m-ko-docs-tasks-debug-application-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-debug-application-cluster"><span>모니터링, 로깅, 그리고 디버깅</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-debug-running-pod-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-debug-running-pod-check">
<label for="m-ko-docs-tasks-debug-application-cluster-debug-running-pod-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/debug-running-pod/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-debug-running-pod"><span>동작 중인 파드 디버그</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-get-shell-running-container-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-get-shell-running-container-check">
<label for="m-ko-docs-tasks-debug-application-cluster-get-shell-running-container-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/get-shell-running-container/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-get-shell-running-container"><span>동작중인 컨테이너의 셸에 접근하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-resource-metrics-pipeline-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-resource-metrics-pipeline-check">
<label for="m-ko-docs-tasks-debug-application-cluster-resource-metrics-pipeline-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/resource-metrics-pipeline/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-resource-metrics-pipeline"><span>리소스 메트릭 파이프라인</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-resource-usage-monitoring-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-resource-usage-monitoring-check">
<label for="m-ko-docs-tasks-debug-application-cluster-resource-usage-monitoring-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/resource-usage-monitoring/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-resource-usage-monitoring"><span>리소스 모니터링 도구</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-debug-stateful-set-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-debug-stateful-set-check">
<label for="m-ko-docs-tasks-debug-application-cluster-debug-stateful-set-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/debug-stateful-set/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-debug-stateful-set"><span>스테이트풀셋 디버깅하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-debug-init-containers-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-debug-init-containers-check">
<label for="m-ko-docs-tasks-debug-application-cluster-debug-init-containers-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/debug-init-containers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-debug-init-containers"><span>초기화 컨테이너(Init Containers) 디버그하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-debug-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-debug-cluster-check">
<label for="m-ko-docs-tasks-debug-application-cluster-debug-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/debug-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-debug-cluster"><span>클러스터 트러블슈팅</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-troubleshooting-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-troubleshooting-check">
<label for="m-ko-docs-tasks-debug-application-cluster-troubleshooting-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/troubleshooting/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-troubleshooting"><span>트러블슈팅하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-determine-reason-pod-failure-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-determine-reason-pod-failure-check">
<label for="m-ko-docs-tasks-debug-application-cluster-determine-reason-pod-failure-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/determine-reason-pod-failure/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-determine-reason-pod-failure"><span>파드 실패의 원인 검증하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-debug-application-cluster-debug-pod-replication-controller-li">
<input type="checkbox" id="m-ko-docs-tasks-debug-application-cluster-debug-pod-replication-controller-check">
<label for="m-ko-docs-tasks-debug-application-cluster-debug-pod-replication-controller-check"><a href="https://kubernetes.io/ko/docs/tasks/debug-application-cluster/debug-pod-replication-controller/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-debug-application-cluster-debug-pod-replication-controller"><span>파드와 레플리케이션컨트롤러(ReplicationController) 디버그하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-extend-kubernetes-li">
<input type="checkbox" id="m-ko-docs-tasks-extend-kubernetes-check">
<label for="m-ko-docs-tasks-extend-kubernetes-check"><a href="https://kubernetes.io/ko/docs/tasks/extend-kubernetes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-extend-kubernetes"><span>쿠버네티스 확장</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-extend-kubernetes-configure-aggregation-layer-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-configure-aggregation-layer-check">
<label for="m-docs-tasks-extend-kubernetes-configure-aggregation-layer-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/configure-aggregation-layer/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-extend-kubernetes-configure-aggregation-layer"><span>Configure the Aggregation Layer</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-tasks-extend-kubernetes-custom-resources-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-custom-resources-check">
<label for="m-docs-tasks-extend-kubernetes-custom-resources-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-tasks-extend-kubernetes-custom-resources"><span>Use Custom Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definitions-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definitions-check">
<label for="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definitions-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definitions"><span>Extend the Kubernetes API with CustomResourceDefinitions</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definition-versioning-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definition-versioning-check">
<label for="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definition-versioning-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-extend-kubernetes-custom-resources-custom-resource-definition-versioning"><span>Versions in CustomResourceDefinitions</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-extend-kubernetes-setup-extension-api-server-li">
<input type="checkbox" id="m-ko-docs-tasks-extend-kubernetes-setup-extension-api-server-check">
<label for="m-ko-docs-tasks-extend-kubernetes-setup-extension-api-server-check"><a href="https://kubernetes.io/ko/docs/tasks/extend-kubernetes/setup-extension-api-server/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-extend-kubernetes-setup-extension-api-server"><span>확장 API 서버 설정</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-extend-kubernetes-configure-multiple-schedulers-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-configure-multiple-schedulers-check">
<label for="m-docs-tasks-extend-kubernetes-configure-multiple-schedulers-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-extend-kubernetes-configure-multiple-schedulers"><span>Configure Multiple Schedulers</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-extend-kubernetes-http-proxy-access-api-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-http-proxy-access-api-check">
<label for="m-docs-tasks-extend-kubernetes-http-proxy-access-api-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-extend-kubernetes-http-proxy-access-api"><span>Use an HTTP Proxy to Access the Kubernetes API</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-extend-kubernetes-socks5-proxy-access-api-li">
<input type="checkbox" id="m-docs-tasks-extend-kubernetes-socks5-proxy-access-api-check">
<label for="m-docs-tasks-extend-kubernetes-socks5-proxy-access-api-check"><a href="https://kubernetes.io/docs/tasks/extend-kubernetes/socks5-proxy-access-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-extend-kubernetes-socks5-proxy-access-api"><span>Use a SOCKS5 Proxy to Access the Kubernetes API</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-extend-kubernetes-setup-konnectivity-li">
<input type="checkbox" id="m-ko-docs-tasks-extend-kubernetes-setup-konnectivity-check">
<label for="m-ko-docs-tasks-extend-kubernetes-setup-konnectivity-check"><a href="https://kubernetes.io/ko/docs/tasks/extend-kubernetes/setup-konnectivity/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-extend-kubernetes-setup-konnectivity"><span>Konnectivity 서비스 설정</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-tls-li">
<input type="checkbox" id="m-ko-docs-tasks-tls-check">
<label for="m-ko-docs-tasks-tls-check"><a href="https://kubernetes.io/ko/docs/tasks/tls/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-tls"><span>TLS</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-tls-certificate-rotation-li">
<input type="checkbox" id="m-ko-docs-tasks-tls-certificate-rotation-check">
<label for="m-ko-docs-tasks-tls-certificate-rotation-check"><a href="https://kubernetes.io/ko/docs/tasks/tls/certificate-rotation/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-tls-certificate-rotation"><span>Kubelet의 인증서 갱신 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-tls-manual-rotation-of-ca-certificates-li">
<input type="checkbox" id="m-docs-tasks-tls-manual-rotation-of-ca-certificates-check">
<label for="m-docs-tasks-tls-manual-rotation-of-ca-certificates-check"><a href="https://kubernetes.io/docs/tasks/tls/manual-rotation-of-ca-certificates/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-tls-manual-rotation-of-ca-certificates"><span>Manual Rotation of CA Certificates</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-tls-managing-tls-in-a-cluster-li">
<input type="checkbox" id="m-ko-docs-tasks-tls-managing-tls-in-a-cluster-check">
<label for="m-ko-docs-tasks-tls-managing-tls-in-a-cluster-check"><a href="https://kubernetes.io/ko/docs/tasks/tls/managing-tls-in-a-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-tls-managing-tls-in-a-cluster"><span>클러스터에서 TLS 인증서 관리</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-manage-daemon-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-daemon-check">
<label for="m-ko-docs-tasks-manage-daemon-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-daemon/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-manage-daemon"><span>클러스터 데몬 관리</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-daemon-update-daemon-set-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-daemon-update-daemon-set-check">
<label for="m-ko-docs-tasks-manage-daemon-update-daemon-set-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-daemon/update-daemon-set/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-daemon-update-daemon-set"><span>데몬셋(DaemonSet)에서 롤링 업데이트 수행</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-daemon-rollback-daemon-set-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-daemon-rollback-daemon-set-check">
<label for="m-ko-docs-tasks-manage-daemon-rollback-daemon-set-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-daemon/rollback-daemon-set/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-daemon-rollback-daemon-set"><span>데몬셋(DaemonSet)에서 롤백 수행</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-service-catalog-li">
<input type="checkbox" id="m-ko-docs-tasks-service-catalog-check">
<label for="m-ko-docs-tasks-service-catalog-check"><a href="https://kubernetes.io/ko/docs/tasks/service-catalog/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-service-catalog"><span>서비스 카탈로그</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-service-catalog-install-service-catalog-using-helm-li">
<input type="checkbox" id="m-docs-tasks-service-catalog-install-service-catalog-using-helm-check">
<label for="m-docs-tasks-service-catalog-install-service-catalog-using-helm-check"><a href="https://kubernetes.io/docs/tasks/service-catalog/install-service-catalog-using-helm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-service-catalog-install-service-catalog-using-helm"><span>Install Service Catalog using Helm</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-service-catalog-install-service-catalog-using-sc-li">
<input type="checkbox" id="m-ko-docs-tasks-service-catalog-install-service-catalog-using-sc-check">
<label for="m-ko-docs-tasks-service-catalog-install-service-catalog-using-sc-check"><a href="https://kubernetes.io/ko/docs/tasks/service-catalog/install-service-catalog-using-sc/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-service-catalog-install-service-catalog-using-sc"><span>SC로 서비스 카탈로그 설치하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tasks-network-li">
<input type="checkbox" id="m-ko-docs-tasks-network-check">
<label for="m-ko-docs-tasks-network-check"><a href="https://kubernetes.io/ko/docs/tasks/network/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tasks-network"><span>네트워킹</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-network-customize-hosts-file-for-pods-li">
<input type="checkbox" id="m-ko-docs-tasks-network-customize-hosts-file-for-pods-check">
<label for="m-ko-docs-tasks-network-customize-hosts-file-for-pods-check"><a href="https://kubernetes.io/ko/docs/tasks/network/customize-hosts-file-for-pods/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-network-customize-hosts-file-for-pods"><span>HostAliases로 파드의 /etc/hosts 항목 추가하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-network-validate-dual-stack-li">
<input type="checkbox" id="m-ko-docs-tasks-network-validate-dual-stack-check">
<label for="m-ko-docs-tasks-network-validate-dual-stack-check"><a href="https://kubernetes.io/ko/docs/tasks/network/validate-dual-stack/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-network-validate-dual-stack"><span>IPv4/IPv6 이중 스택 검증</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tasks-kubelet-credential-provider-kubelet-credential-provider-li">
<input type="checkbox" id="m-docs-tasks-kubelet-credential-provider-kubelet-credential-provider-check">
<label for="m-docs-tasks-kubelet-credential-provider-kubelet-credential-provider-check"><a href="https://kubernetes.io/docs/tasks/kubelet-credential-provider/kubelet-credential-provider/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tasks-kubelet-credential-provider-kubelet-credential-provider"><span>Configure a kubelet image credential provider</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-gpus-scheduling-gpus-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-gpus-scheduling-gpus-check">
<label for="m-ko-docs-tasks-manage-gpus-scheduling-gpus-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-gpus/scheduling-gpus/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-gpus-scheduling-gpus"><span>GPU 스케줄링</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-manage-hugepages-scheduling-hugepages-li">
<input type="checkbox" id="m-ko-docs-tasks-manage-hugepages-scheduling-hugepages-check">
<label for="m-ko-docs-tasks-manage-hugepages-scheduling-hugepages-check"><a href="https://kubernetes.io/ko/docs/tasks/manage-hugepages/scheduling-hugepages/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-manage-hugepages-scheduling-hugepages"><span>HugePages 관리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tasks-extend-kubectl-kubectl-plugins-li">
<input type="checkbox" id="m-ko-docs-tasks-extend-kubectl-kubectl-plugins-check">
<label for="m-ko-docs-tasks-extend-kubectl-kubectl-plugins-check"><a href="https://kubernetes.io/ko/docs/tasks/extend-kubectl/kubectl-plugins/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tasks-extend-kubectl-kubectl-plugins"><span>플러그인으로 kubectl 확장</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-li">
<input type="checkbox" id="m-ko-docs-tutorials-check">
<label for="m-ko-docs-tutorials-check"><a href="https://kubernetes.io/ko/docs/tutorials/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials"><span>튜토리얼</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-hello-minikube-li">
<input type="checkbox" id="m-ko-docs-tutorials-hello-minikube-check">
<label for="m-ko-docs-tutorials-hello-minikube-check"><a href="https://kubernetes.io/ko/docs/tutorials/hello-minikube/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-hello-minikube"><span>Hello Minikube</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics"><span>쿠버네티스 기초 학습</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-create-cluster-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster"><span>클러스터 생성하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-intro-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-intro-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-intro-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-intro"><span>Minikube를 사용해서 클러스터 생성하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-interactive-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-create-cluster-cluster-interactive"><span>대화형 튜토리얼 - 클러스터 생성하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-deploy-app-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/deploy-app/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app"><span>앱 배포하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-intro-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-intro-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-intro-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-intro"><span>kubectl을 사용해서 디플로이먼트 생성하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-interactive-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/deploy-app/deploy-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-deploy-app-deploy-interactive"><span>대화형 튜토리얼 - 앱 배포하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-explore-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-explore-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-explore-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/explore/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics-explore"><span>앱 조사하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-explore-explore-intro-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-explore-explore-intro-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-explore-explore-intro-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/explore/explore-intro/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-explore-explore-intro"><span>파드와 노드 보기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-explore-explore-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-explore-explore-interactive-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-explore-explore-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/explore/explore-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-explore-explore-interactive"><span>대화형 튜토리얼 - 앱 조사하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-expose-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-expose-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-expose-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/expose/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics-expose"><span>앱 외부로 노출하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-expose-expose-intro-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-expose-expose-intro-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-expose-expose-intro-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/expose/expose-intro/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-expose-expose-intro"><span>앱 노출을 위해 서비스 이용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-expose-expose-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-expose-expose-interactive-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-expose-expose-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/expose/expose-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-expose-expose-interactive"><span>대화형 튜토리얼 - 앱 노출하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-scale-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-scale-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-scale-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/scale/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics-scale"><span>앱 스케일링하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-scale-scale-intro-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-scale-scale-intro-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-scale-scale-intro-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/scale/scale-intro/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-scale-scale-intro"><span>복수의 앱 인스턴스를 구동하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-scale-scale-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-scale-scale-interactive-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-scale-scale-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/scale/scale-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-scale-scale-interactive"><span>대화형 튜토리얼 - 앱 스케일링하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-kubernetes-basics-update-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-update-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-update-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/update/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-kubernetes-basics-update"><span>앱 업데이트하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-update-update-intro-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-update-update-intro-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-update-update-intro-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/update/update-intro/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-update-update-intro"><span>롤링 업데이트 수행하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-kubernetes-basics-update-update-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-kubernetes-basics-update-update-interactive-check">
<label for="m-ko-docs-tutorials-kubernetes-basics-update-update-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/update/update-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-kubernetes-basics-update-update-interactive"><span>대화형 튜토리얼 - 앱 업데이트 하기</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-configuration-li">
<input type="checkbox" id="m-ko-docs-tutorials-configuration-check">
<label for="m-ko-docs-tutorials-configuration-check"><a href="https://kubernetes.io/ko/docs/tutorials/configuration/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-configuration"><span>설정</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-configuration-configure-java-microservice-li">
<input type="checkbox" id="m-ko-docs-tutorials-configuration-configure-java-microservice-check">
<label for="m-ko-docs-tutorials-configuration-configure-java-microservice-check"><a href="https://kubernetes.io/ko/docs/tutorials/configuration/configure-java-microservice/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-configuration-configure-java-microservice"><span>예제: Java 마이크로서비스 구성하기</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-li">
<input type="checkbox" id="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-check">
<label for="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-check"><a href="https://kubernetes.io/ko/docs/tutorials/configuration/configure-java-microservice/configure-java-microservice/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice"><span>MicroProfile, 컨피그맵(ConfigMaps) 및 시크릿(Secrets)을 사용하여 구성 외부화(externalizing)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-interactive-li">
<input type="checkbox" id="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-interactive-check">
<label for="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-interactive-check"><a href="https://kubernetes.io/ko/docs/tutorials/configuration/configure-java-microservice/configure-java-microservice-interactive/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-configuration-configure-java-microservice-configure-java-microservice-interactive"><span>대화형 튜토리얼 - Java 마이크로서비스 구성하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-configuration-configure-redis-using-configmap-li">
<input type="checkbox" id="m-ko-docs-tutorials-configuration-configure-redis-using-configmap-check">
<label for="m-ko-docs-tutorials-configuration-configure-redis-using-configmap-check"><a href="https://kubernetes.io/ko/docs/tutorials/configuration/configure-redis-using-configmap/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-configuration-configure-redis-using-configmap"><span>컨피그맵을 사용해서 Redis 설정하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-security-li">
<input type="checkbox" id="m-ko-docs-tutorials-security-check">
<label for="m-ko-docs-tutorials-security-check"><a href="https://kubernetes.io/ko/docs/tutorials/security/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-security"><span>보안</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-security-apparmor-li">
<input type="checkbox" id="m-ko-docs-tutorials-security-apparmor-check">
<label for="m-ko-docs-tutorials-security-apparmor-check"><a href="https://kubernetes.io/ko/docs/tutorials/security/apparmor/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-security-apparmor"><span>AppArmor를 사용하여 리소스에 대한 컨테이너의 접근 제한</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-security-ns-level-pss-li">
<input type="checkbox" id="m-ko-docs-tutorials-security-ns-level-pss-check">
<label for="m-ko-docs-tutorials-security-ns-level-pss-check"><a href="https://kubernetes.io/ko/docs/tutorials/security/ns-level-pss/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-security-ns-level-pss"><span>파드 시큐리티 스탠다드를 네임스페이스 수준에 적용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-security-cluster-level-pss-li">
<input type="checkbox" id="m-ko-docs-tutorials-security-cluster-level-pss-check">
<label for="m-ko-docs-tutorials-security-cluster-level-pss-check"><a href="https://kubernetes.io/ko/docs/tutorials/security/cluster-level-pss/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-security-cluster-level-pss"><span>파드 시큐리티 스탠다드를 클러스터 수준에 적용하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-tutorials-security-seccomp-li">
<input type="checkbox" id="m-docs-tutorials-security-seccomp-check">
<label for="m-docs-tutorials-security-seccomp-check"><a href="https://kubernetes.io/docs/tutorials/security/seccomp/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-tutorials-security-seccomp"><span>Restrict a Container's Syscalls with seccomp</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-stateless-application-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateless-application-check">
<label for="m-ko-docs-tutorials-stateless-application-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateless-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-stateless-application"><span>상태 유지를 하지 않는 애플리케이션</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-stateless-application-expose-external-ip-address-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateless-application-expose-external-ip-address-check">
<label for="m-ko-docs-tutorials-stateless-application-expose-external-ip-address-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateless-application/expose-external-ip-address/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-stateless-application-expose-external-ip-address"><span>외부 IP 주소를 노출하여 클러스터의 애플리케이션에 접속하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-stateless-application-guestbook-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateless-application-guestbook-check">
<label for="m-ko-docs-tutorials-stateless-application-guestbook-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateless-application/guestbook/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-stateless-application-guestbook"><span>예시: Redis를 사용한 PHP 방명록 애플리케이션 배포하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-stateful-application-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateful-application-check">
<label for="m-ko-docs-tutorials-stateful-application-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateful-application/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-stateful-application"><span>상태 유지가 필요한(stateful) 애플리케이션</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-stateful-application-basic-stateful-set-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateful-application-basic-stateful-set-check">
<label for="m-ko-docs-tutorials-stateful-application-basic-stateful-set-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateful-application/basic-stateful-set/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-stateful-application-basic-stateful-set"><span>스테이트풀셋 기본</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-stateful-application-mysql-wordpress-persistent-volume-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateful-application-mysql-wordpress-persistent-volume-check">
<label for="m-ko-docs-tutorials-stateful-application-mysql-wordpress-persistent-volume-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-stateful-application-mysql-wordpress-persistent-volume"><span>예시: WordPress와 MySQL을 퍼시스턴트 볼륨에 배포하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-stateful-application-cassandra-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateful-application-cassandra-check">
<label for="m-ko-docs-tutorials-stateful-application-cassandra-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateful-application/cassandra/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-stateful-application-cassandra"><span>예시: 카산드라를 스테이트풀셋으로 배포하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-stateful-application-zookeeper-li">
<input type="checkbox" id="m-ko-docs-tutorials-stateful-application-zookeeper-check">
<label for="m-ko-docs-tutorials-stateful-application-zookeeper-check"><a href="https://kubernetes.io/ko/docs/tutorials/stateful-application/zookeeper/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-stateful-application-zookeeper"><span>분산 시스템 코디네이터 ZooKeeper 실행하기</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-tutorials-services-li">
<input type="checkbox" id="m-ko-docs-tutorials-services-check">
<label for="m-ko-docs-tutorials-services-check"><a href="https://kubernetes.io/ko/docs/tutorials/services/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-tutorials-services"><span>서비스</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-tutorials-services-source-ip-li">
<input type="checkbox" id="m-ko-docs-tutorials-services-source-ip-check">
<label for="m-ko-docs-tutorials-services-source-ip-check"><a href="https://kubernetes.io/ko/docs/tutorials/services/source-ip/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-tutorials-services-source-ip"><span>소스 IP 주소 이용하기</span></a></label>
</li>
</ul>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-li">
<input type="checkbox" id="m-ko-docs-reference-check">
<label for="m-ko-docs-reference-check"><a href="https://kubernetes.io/ko/docs/reference/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference"><span>레퍼런스</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-glossary-li">
<input type="checkbox" id="m-ko-docs-reference-glossary-check">
<label for="m-ko-docs-reference-glossary-check"><a href="https://kubernetes.io/ko/docs/reference/glossary/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-glossary"><span>용어집</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-using-api-li">
<input type="checkbox" id="m-ko-docs-reference-using-api-check">
<label for="m-ko-docs-reference-using-api-check"><a href="https://kubernetes.io/ko/docs/reference/using-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-using-api"><span>API 개요</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-using-api-api-concepts-li">
<input type="checkbox" id="m-docs-reference-using-api-api-concepts-check">
<label for="m-docs-reference-using-api-api-concepts-check"><a href="https://kubernetes.io/docs/reference/using-api/api-concepts/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-using-api-api-concepts"><span>Kubernetes API Concepts</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-using-api-server-side-apply-li">
<input type="checkbox" id="m-docs-reference-using-api-server-side-apply-check">
<label for="m-docs-reference-using-api-server-side-apply-check"><a href="https://kubernetes.io/docs/reference/using-api/server-side-apply/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-using-api-server-side-apply"><span>Server-Side Apply</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-using-api-client-libraries-li">
<input type="checkbox" id="m-ko-docs-reference-using-api-client-libraries-check">
<label for="m-ko-docs-reference-using-api-client-libraries-check"><a href="https://kubernetes.io/ko/docs/reference/using-api/client-libraries/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-using-api-client-libraries"><span>클라이언트 라이브러리</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-using-api-deprecation-policy-li">
<input type="checkbox" id="m-docs-reference-using-api-deprecation-policy-check">
<label for="m-docs-reference-using-api-deprecation-policy-check"><a href="https://kubernetes.io/docs/reference/using-api/deprecation-policy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-using-api-deprecation-policy"><span>Kubernetes Deprecation Policy</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-using-api-deprecation-guide-li">
<input type="checkbox" id="m-docs-reference-using-api-deprecation-guide-check">
<label for="m-docs-reference-using-api-deprecation-guide-check"><a href="https://kubernetes.io/docs/reference/using-api/deprecation-guide/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-using-api-deprecation-guide"><span>Deprecated API Migration Guide</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-using-api-health-checks-li">
<input type="checkbox" id="m-ko-docs-reference-using-api-health-checks-check">
<label for="m-ko-docs-reference-using-api-health-checks-check"><a href="https://kubernetes.io/ko/docs/reference/using-api/health-checks/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-using-api-health-checks"><span>쿠버네티스 API 헬스(health) 엔드포인트</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-access-authn-authz-li">
<input type="checkbox" id="m-ko-docs-reference-access-authn-authz-check">
<label for="m-ko-docs-reference-access-authn-authz-check"><a href="https://kubernetes.io/ko/docs/reference/access-authn-authz/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-access-authn-authz"><span>API 접근 제어</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-authentication-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-authentication-check">
<label for="m-docs-reference-access-authn-authz-authentication-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-authentication"><span>Authenticating</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-bootstrap-tokens-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-bootstrap-tokens-check">
<label for="m-docs-reference-access-authn-authz-bootstrap-tokens-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/bootstrap-tokens/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-bootstrap-tokens"><span>Authenticating with Bootstrap Tokens</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-certificate-signing-requests-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-certificate-signing-requests-check">
<label for="m-docs-reference-access-authn-authz-certificate-signing-requests-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-certificate-signing-requests"><span>Certificate Signing Requests</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-admission-controllers-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-admission-controllers-check">
<label for="m-docs-reference-access-authn-authz-admission-controllers-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-admission-controllers"><span>Using Admission Controllers</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-extensible-admission-controllers-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-extensible-admission-controllers-check">
<label for="m-docs-reference-access-authn-authz-extensible-admission-controllers-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-extensible-admission-controllers"><span>Dynamic Admission Control</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-access-authn-authz-service-accounts-admin-li">
<input type="checkbox" id="m-ko-docs-reference-access-authn-authz-service-accounts-admin-check">
<label for="m-ko-docs-reference-access-authn-authz-service-accounts-admin-check"><a href="https://kubernetes.io/ko/docs/reference/access-authn-authz/service-accounts-admin/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-access-authn-authz-service-accounts-admin"><span>서비스 어카운트 관리하기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-access-authn-authz-authorization-li">
<input type="checkbox" id="m-ko-docs-reference-access-authn-authz-authorization-check">
<label for="m-ko-docs-reference-access-authn-authz-authorization-check"><a href="https://kubernetes.io/ko/docs/reference/access-authn-authz/authorization/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-access-authn-authz-authorization"><span>인가 개요</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-rbac-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-rbac-check">
<label for="m-docs-reference-access-authn-authz-rbac-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-rbac"><span>Using RBAC Authorization</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-abac-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-abac-check">
<label for="m-docs-reference-access-authn-authz-abac-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/abac/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-abac"><span>Using ABAC Authorization</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-node-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-node-check">
<label for="m-docs-reference-access-authn-authz-node-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/node/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-node"><span>Using Node Authorization</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-psp-to-pod-security-standards-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-psp-to-pod-security-standards-check">
<label for="m-docs-reference-access-authn-authz-psp-to-pod-security-standards-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/psp-to-pod-security-standards/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-psp-to-pod-security-standards"><span>Mapping PodSecurityPolicies to Pod Security Standards</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-access-authn-authz-webhook-li">
<input type="checkbox" id="m-docs-reference-access-authn-authz-webhook-check">
<label for="m-docs-reference-access-authn-authz-webhook-check"><a href="https://kubernetes.io/docs/reference/access-authn-authz/webhook/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-access-authn-authz-webhook"><span>Webhook Mode</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-labels-annotations-taints-li">
<input type="checkbox" id="m-docs-reference-labels-annotations-taints-check">
<label for="m-docs-reference-labels-annotations-taints-check"><a href="https://kubernetes.io/docs/reference/labels-annotations-taints/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-labels-annotations-taints"><span>Well-Known Labels, Annotations and Taints</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-labels-annotations-taints-audit-annotations-li">
<input type="checkbox" id="m-docs-reference-labels-annotations-taints-audit-annotations-check">
<label for="m-docs-reference-labels-annotations-taints-audit-annotations-check"><a href="https://kubernetes.io/docs/reference/labels-annotations-taints/audit-annotations/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-labels-annotations-taints-audit-annotations"><span>Audit Annotations</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-labels-annotations-taints-li">
<input type="checkbox" id="m-ko-docs-reference-labels-annotations-taints-check">
<label for="m-ko-docs-reference-labels-annotations-taints-check"><a href="https://kubernetes.io/ko/docs/reference/labels-annotations-taints/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-labels-annotations-taints"><span>잘 알려진 레이블, 어노테이션, 테인트(Taint)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-check">
<label for="m-docs-reference-kubernetes-api-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api"><span>Kubernetes API</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-workload-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-workload-resources"><span>Workload Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-pod-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-pod-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-pod-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-pod-v1"><span>Pod</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-pod-template-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-pod-template-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-pod-template-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-template-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-pod-template-v1"><span>PodTemplate</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-replication-controller-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-replication-controller-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-replication-controller-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/replication-controller-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-replication-controller-v1"><span>ReplicationController</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-replica-set-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-replica-set-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-replica-set-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/replica-set-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-replica-set-v1"><span>ReplicaSet</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-deployment-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-deployment-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-deployment-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-deployment-v1"><span>Deployment</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-stateful-set-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-stateful-set-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-stateful-set-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/stateful-set-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-stateful-set-v1"><span>StatefulSet</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-controller-revision-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-controller-revision-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-controller-revision-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/controller-revision-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-controller-revision-v1"><span>ControllerRevision</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-daemon-set-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-daemon-set-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-daemon-set-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/daemon-set-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-daemon-set-v1"><span>DaemonSet</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-job-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-job-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-job-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/job-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-job-v1"><span>Job</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-cron-job-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-cron-job-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-cron-job-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/cron-job-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-cron-job-v1"><span>CronJob</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/horizontal-pod-autoscaler-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v1"><span>HorizontalPodAutoscaler</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/horizontal-pod-autoscaler-v2/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2"><span>HorizontalPodAutoscaler</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2beta2-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2beta2-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2beta2-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/horizontal-pod-autoscaler-v2beta2/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-horizontal-pod-autoscaler-v2beta2"><span>HorizontalPodAutoscaler v2beta2</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-workload-resources-priority-class-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-workload-resources-priority-class-v1-check">
<label for="m-docs-reference-kubernetes-api-workload-resources-priority-class-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/priority-class-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-workload-resources-priority-class-v1"><span>PriorityClass</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-service-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-service-resources-check">
<label for="m-docs-reference-kubernetes-api-service-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-service-resources"><span>Service Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-service-resources-service-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-service-resources-service-v1-check">
<label for="m-docs-reference-kubernetes-api-service-resources-service-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/service-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-service-resources-service-v1"><span>Service</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-service-resources-endpoints-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-service-resources-endpoints-v1-check">
<label for="m-docs-reference-kubernetes-api-service-resources-endpoints-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/endpoints-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-service-resources-endpoints-v1"><span>Endpoints</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-service-resources-endpoint-slice-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-service-resources-endpoint-slice-v1-check">
<label for="m-docs-reference-kubernetes-api-service-resources-endpoint-slice-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/endpoint-slice-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-service-resources-endpoint-slice-v1"><span>EndpointSlice</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-service-resources-ingress-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-service-resources-ingress-v1-check">
<label for="m-docs-reference-kubernetes-api-service-resources-ingress-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-service-resources-ingress-v1"><span>Ingress</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-service-resources-ingress-class-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-service-resources-ingress-class-v1-check">
<label for="m-docs-reference-kubernetes-api-service-resources-ingress-class-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-class-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-service-resources-ingress-class-v1"><span>IngressClass</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-config-and-storage-resources"><span>Config and Storage Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-config-map-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-config-map-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-config-map-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/config-map-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-config-map-v1"><span>ConfigMap</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-secret-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-secret-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-secret-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/secret-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-secret-v1"><span>Secret</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/volume/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-volume"><span>Volume</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-claim-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-claim-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-claim-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/persistent-volume-claim-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-claim-v1"><span>PersistentVolumeClaim</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/persistent-volume-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-persistent-volume-v1"><span>PersistentVolume</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-storage-class-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-storage-class-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-storage-class-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/storage-class-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-storage-class-v1"><span>StorageClass</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-attachment-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-attachment-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-attachment-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/volume-attachment-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-volume-attachment-v1"><span>VolumeAttachment</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-driver-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-driver-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-driver-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/csi-driver-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-driver-v1"><span>CSIDriver</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-node-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-node-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-node-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/csi-node-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-node-v1"><span>CSINode</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-storage-capacity-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-storage-capacity-v1-check">
<label for="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-storage-capacity-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/csi-storage-capacity-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-config-and-storage-resources-csi-storage-capacity-v1"><span>CSIStorageCapacity</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-authentication-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authentication-resources-check">
<label for="m-docs-reference-kubernetes-api-authentication-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-authentication-resources"><span>Authentication Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authentication-resources-service-account-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authentication-resources-service-account-v1-check">
<label for="m-docs-reference-kubernetes-api-authentication-resources-service-account-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/service-account-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authentication-resources-service-account-v1"><span>ServiceAccount</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authentication-resources-token-request-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authentication-resources-token-request-v1-check">
<label for="m-docs-reference-kubernetes-api-authentication-resources-token-request-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/token-request-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authentication-resources-token-request-v1"><span>TokenRequest</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authentication-resources-token-review-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authentication-resources-token-review-v1-check">
<label for="m-docs-reference-kubernetes-api-authentication-resources-token-review-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/token-review-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authentication-resources-token-review-v1"><span>TokenReview</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authentication-resources-certificate-signing-request-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authentication-resources-certificate-signing-request-v1-check">
<label for="m-docs-reference-kubernetes-api-authentication-resources-certificate-signing-request-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/certificate-signing-request-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authentication-resources-certificate-signing-request-v1"><span>CertificateSigningRequest</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-authorization-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-authorization-resources"><span>Authorization Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-local-subject-access-review-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-local-subject-access-review-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-local-subject-access-review-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/local-subject-access-review-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-local-subject-access-review-v1"><span>LocalSubjectAccessReview</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-self-subject-access-review-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-self-subject-access-review-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-self-subject-access-review-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/self-subject-access-review-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-self-subject-access-review-v1"><span>SelfSubjectAccessReview</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-self-subject-rules-review-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-self-subject-rules-review-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-self-subject-rules-review-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/self-subject-rules-review-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-self-subject-rules-review-v1"><span>SelfSubjectRulesReview</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-subject-access-review-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-subject-access-review-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-subject-access-review-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/subject-access-review-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-subject-access-review-v1"><span>SubjectAccessReview</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/cluster-role-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-v1"><span>ClusterRole</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-binding-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-binding-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-binding-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/cluster-role-binding-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-cluster-role-binding-v1"><span>ClusterRoleBinding</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-role-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-role-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-role-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/role-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-role-v1"><span>Role</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-authorization-resources-role-binding-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-authorization-resources-role-binding-v1-check">
<label for="m-docs-reference-kubernetes-api-authorization-resources-role-binding-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/authorization-resources/role-binding-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-authorization-resources-role-binding-v1"><span>RoleBinding</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-policy-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-policy-resources-check">
<label for="m-docs-reference-kubernetes-api-policy-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-policy-resources"><span>Policy Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-policy-resources-limit-range-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-policy-resources-limit-range-v1-check">
<label for="m-docs-reference-kubernetes-api-policy-resources-limit-range-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/limit-range-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-policy-resources-limit-range-v1"><span>LimitRange</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-policy-resources-resource-quota-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-policy-resources-resource-quota-v1-check">
<label for="m-docs-reference-kubernetes-api-policy-resources-resource-quota-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/resource-quota-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-policy-resources-resource-quota-v1"><span>ResourceQuota</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-policy-resources-network-policy-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-policy-resources-network-policy-v1-check">
<label for="m-docs-reference-kubernetes-api-policy-resources-network-policy-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/network-policy-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-policy-resources-network-policy-v1"><span>NetworkPolicy</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-policy-resources-pod-disruption-budget-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-policy-resources-pod-disruption-budget-v1-check">
<label for="m-docs-reference-kubernetes-api-policy-resources-pod-disruption-budget-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/pod-disruption-budget-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-policy-resources-pod-disruption-budget-v1"><span>PodDisruptionBudget</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-policy-resources-pod-security-policy-v1beta1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-policy-resources-pod-security-policy-v1beta1-check">
<label for="m-docs-reference-kubernetes-api-policy-resources-pod-security-policy-v1beta1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/pod-security-policy-v1beta1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-policy-resources-pod-security-policy-v1beta1"><span>PodSecurityPolicy v1beta1</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-extend-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-extend-resources-check">
<label for="m-docs-reference-kubernetes-api-extend-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/extend-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-extend-resources"><span>Extend Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-extend-resources-custom-resource-definition-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-extend-resources-custom-resource-definition-v1-check">
<label for="m-docs-reference-kubernetes-api-extend-resources-custom-resource-definition-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/extend-resources/custom-resource-definition-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-extend-resources-custom-resource-definition-v1"><span>CustomResourceDefinition</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-extend-resources-mutating-webhook-configuration-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-extend-resources-mutating-webhook-configuration-v1-check">
<label for="m-docs-reference-kubernetes-api-extend-resources-mutating-webhook-configuration-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/extend-resources/mutating-webhook-configuration-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-extend-resources-mutating-webhook-configuration-v1"><span>MutatingWebhookConfiguration</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-extend-resources-validating-webhook-configuration-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-extend-resources-validating-webhook-configuration-v1-check">
<label for="m-docs-reference-kubernetes-api-extend-resources-validating-webhook-configuration-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/extend-resources/validating-webhook-configuration-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-extend-resources-validating-webhook-configuration-v1"><span>ValidatingWebhookConfiguration</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-cluster-resources-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-cluster-resources"><span>Cluster Resources</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-node-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-node-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-node-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/node-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-node-v1"><span>Node</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-namespace-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-namespace-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-namespace-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/namespace-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-namespace-v1"><span>Namespace</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-event-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-event-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-event-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/event-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-event-v1"><span>Event</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-api-service-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-api-service-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-api-service-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/api-service-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-api-service-v1"><span>APIService</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-lease-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-lease-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-lease-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/lease-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-lease-v1"><span>Lease</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-runtime-class-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-runtime-class-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-runtime-class-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/runtime-class-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-runtime-class-v1"><span>RuntimeClass</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-flow-schema-v1beta2-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-flow-schema-v1beta2-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-flow-schema-v1beta2-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/flow-schema-v1beta2/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-flow-schema-v1beta2"><span>FlowSchema v1beta2</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-priority-level-configuration-v1beta2-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-priority-level-configuration-v1beta2-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-priority-level-configuration-v1beta2-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/priority-level-configuration-v1beta2/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-priority-level-configuration-v1beta2"><span>PriorityLevelConfiguration v1beta2</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-binding-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-binding-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-binding-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/binding-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-binding-v1"><span>Binding</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-cluster-resources-component-status-v1-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-cluster-resources-component-status-v1-check">
<label for="m-docs-reference-kubernetes-api-cluster-resources-component-status-v1-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/component-status-v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-cluster-resources-component-status-v1"><span>ComponentStatus</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-kubernetes-api-common-definitions-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-kubernetes-api-common-definitions"><span>Common Definitions</span></a></label>
<ul class="ul-4 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-delete-options-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-delete-options-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-delete-options-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/delete-options/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-delete-options"><span>DeleteOptions</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-label-selector-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-label-selector-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-label-selector-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/label-selector/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-label-selector"><span>LabelSelector</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-list-meta-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-list-meta-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-list-meta-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/list-meta/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-list-meta"><span>ListMeta</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-local-object-reference-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-local-object-reference-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-local-object-reference-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/local-object-reference/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-local-object-reference"><span>LocalObjectReference</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-node-selector-requirement-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-node-selector-requirement-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-node-selector-requirement-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/node-selector-requirement/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-node-selector-requirement"><span>NodeSelectorRequirement</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-object-field-selector-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-object-field-selector-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-object-field-selector-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/object-field-selector/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-object-field-selector"><span>ObjectFieldSelector</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-object-meta-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-object-meta-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-object-meta-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/object-meta/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-object-meta"><span>ObjectMeta</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-object-reference-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-object-reference-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-object-reference-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/object-reference/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-object-reference"><span>ObjectReference</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-patch-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-patch-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-patch-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/patch/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-patch"><span>Patch</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-quantity-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-quantity-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-quantity-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/quantity/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-quantity"><span>Quantity</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-resource-field-selector-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-resource-field-selector-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-resource-field-selector-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/resource-field-selector/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-resource-field-selector"><span>ResourceFieldSelector</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-status-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-status-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-status-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/status/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-status"><span>Status</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-definitions-typed-local-object-reference-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-definitions-typed-local-object-reference-check">
<label for="m-docs-reference-kubernetes-api-common-definitions-typed-local-object-reference-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-definitions/typed-local-object-reference/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-definitions-typed-local-object-reference"><span>TypedLocalObjectReference</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubernetes-api-common-parameters-common-parameters-li">
<input type="checkbox" id="m-docs-reference-kubernetes-api-common-parameters-common-parameters-check">
<label for="m-docs-reference-kubernetes-api-common-parameters-common-parameters-check"><a href="https://kubernetes.io/docs/reference/kubernetes-api/common-parameters/common-parameters/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubernetes-api-common-parameters-common-parameters"><span>Common Parameters</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-node-li">
<input type="checkbox" id="m-docs-reference-node-check">
<label for="m-docs-reference-node-check"><a href="https://kubernetes.io/docs/reference/node/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-node"><span>Node Reference Information</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-node-topics-on-dockershim-and-cri-compatible-runtimes-li">
<input type="checkbox" id="m-docs-reference-node-topics-on-dockershim-and-cri-compatible-runtimes-check">
<label for="m-docs-reference-node-topics-on-dockershim-and-cri-compatible-runtimes-check"><a href="https://kubernetes.io/docs/reference/node/topics-on-dockershim-and-cri-compatible-runtimes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-node-topics-on-dockershim-and-cri-compatible-runtimes"><span>Articles on dockershim Removal and on Using CRI-compatible Runtimes</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-issues-security-li">
<input type="checkbox" id="m-ko-docs-reference-issues-security-check">
<label for="m-ko-docs-reference-issues-security-check"><a href="https://kubernetes.io/ko/docs/reference/issues-security/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-issues-security"><span>쿠버네티스 이슈와 보안</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-issues-security-issues-li">
<input type="checkbox" id="m-ko-docs-reference-issues-security-issues-check">
<label for="m-ko-docs-reference-issues-security-issues-check"><a href="https://kubernetes.io/ko/docs/reference/issues-security/issues/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-issues-security-issues"><span>쿠버네티스 이슈 트래커</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-issues-security-security-li">
<input type="checkbox" id="m-ko-docs-reference-issues-security-security-check">
<label for="m-ko-docs-reference-issues-security-security-check"><a href="https://kubernetes.io/ko/docs/reference/issues-security/security/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-issues-security-security"><span>쿠버네티스 보안과 공개 정보</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-setup-tools-li">
<input type="checkbox" id="m-ko-docs-reference-setup-tools-check">
<label for="m-ko-docs-reference-setup-tools-check"><a href="https://kubernetes.io/ko/docs/reference/setup-tools/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-setup-tools"><span>설치 도구</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-setup-tools-kubeadm-li">
<input type="checkbox" id="m-ko-docs-reference-setup-tools-kubeadm-check">
<label for="m-ko-docs-reference-setup-tools-kubeadm-check"><a href="https://kubernetes.io/ko/docs/reference/setup-tools/kubeadm/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-setup-tools-kubeadm"><span>Kubeadm</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-ports-and-protocols-li">
<input type="checkbox" id="m-ko-docs-reference-ports-and-protocols-check">
<label for="m-ko-docs-reference-ports-and-protocols-check"><a href="https://kubernetes.io/ko/docs/reference/ports-and-protocols/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-ports-and-protocols"><span>포트와 프로토콜</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-kubectl-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-check">
<label for="m-ko-docs-reference-kubectl-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-kubectl"><span>kubectl</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-kubectl-kubectl-cmds-li">
<input type="checkbox" id="m-docs-reference-kubectl-kubectl-cmds-check">
<label for="m-docs-reference-kubectl-kubectl-cmds-check"><a href="https://kubernetes.io/docs/reference/kubectl/kubectl-cmds/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-kubectl-kubectl-cmds"><span>kubectl Commands</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-kubectl-overview-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-overview-check">
<label for="m-ko-docs-reference-kubectl-overview-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/overview/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-kubectl-overview"><span>kubectl 개요</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-kubectl-jsonpath-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-jsonpath-check">
<label for="m-ko-docs-reference-kubectl-jsonpath-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/jsonpath/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-kubectl-jsonpath"><span>JSONPath 지원</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-kubectl-kubectl-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-kubectl-check">
<label for="m-ko-docs-reference-kubectl-kubectl-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/kubectl/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-kubectl-kubectl"><span>kubectl</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-kubectl-conventions-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-conventions-check">
<label for="m-ko-docs-reference-kubectl-conventions-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/conventions/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-kubectl-conventions"><span>kubectl 사용 규칙</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-kubectl-cheatsheet-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-cheatsheet-check">
<label for="m-ko-docs-reference-kubectl-cheatsheet-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-kubectl-cheatsheet"><span>kubectl 치트 시트</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-kubectl-docker-cli-to-kubectl-li">
<input type="checkbox" id="m-ko-docs-reference-kubectl-docker-cli-to-kubectl-check">
<label for="m-ko-docs-reference-kubectl-docker-cli-to-kubectl-check"><a href="https://kubernetes.io/ko/docs/reference/kubectl/docker-cli-to-kubectl/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-kubectl-docker-cli-to-kubectl"><span>도커 사용자를 위한 kubectl</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-command-line-tools-reference-li">
<input type="checkbox" id="m-ko-docs-reference-command-line-tools-reference-check">
<label for="m-ko-docs-reference-command-line-tools-reference-check"><a href="https://kubernetes.io/ko/docs/reference/command-line-tools-reference/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-command-line-tools-reference"><span>컴포넌트 도구</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-command-line-tools-reference-feature-gates-li">
<input type="checkbox" id="m-ko-docs-reference-command-line-tools-reference-feature-gates-check">
<label for="m-ko-docs-reference-command-line-tools-reference-feature-gates-check"><a href="https://kubernetes.io/ko/docs/reference/command-line-tools-reference/feature-gates/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-command-line-tools-reference-feature-gates"><span>기능 게이트</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-command-line-tools-reference-kubelet-li">
<input type="checkbox" id="m-docs-reference-command-line-tools-reference-kubelet-check">
<label for="m-docs-reference-command-line-tools-reference-kubelet-check"><a href="https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-command-line-tools-reference-kubelet"><span>kubelet</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-command-line-tools-reference-kube-apiserver-li">
<input type="checkbox" id="m-docs-reference-command-line-tools-reference-kube-apiserver-check">
<label for="m-docs-reference-command-line-tools-reference-kube-apiserver-check"><a href="https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-command-line-tools-reference-kube-apiserver"><span>kube-apiserver</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-command-line-tools-reference-kube-controller-manager-li">
<input type="checkbox" id="m-docs-reference-command-line-tools-reference-kube-controller-manager-check">
<label for="m-docs-reference-command-line-tools-reference-kube-controller-manager-check"><a href="https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-command-line-tools-reference-kube-controller-manager"><span>kube-controller-manager</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-command-line-tools-reference-kube-proxy-li">
<input type="checkbox" id="m-ko-docs-reference-command-line-tools-reference-kube-proxy-check">
<label for="m-ko-docs-reference-command-line-tools-reference-kube-proxy-check"><a href="https://kubernetes.io/ko/docs/reference/command-line-tools-reference/kube-proxy/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-command-line-tools-reference-kube-proxy"><span>kube-proxy</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-command-line-tools-reference-kube-scheduler-li">
<input type="checkbox" id="m-docs-reference-command-line-tools-reference-kube-scheduler-check">
<label for="m-docs-reference-command-line-tools-reference-kube-scheduler-check"><a href="https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-command-line-tools-reference-kube-scheduler"><span>kube-scheduler</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-command-line-tools-reference-kubelet-authentication-authorization-li">
<input type="checkbox" id="m-ko-docs-reference-command-line-tools-reference-kubelet-authentication-authorization-check">
<label for="m-ko-docs-reference-command-line-tools-reference-kubelet-authentication-authorization-check"><a href="https://kubernetes.io/ko/docs/reference/command-line-tools-reference/kubelet-authentication-authorization/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-command-line-tools-reference-kubelet-authentication-authorization"><span>Kubelet 인증/인가</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-command-line-tools-reference-kubelet-tls-bootstrapping-li">
<input type="checkbox" id="m-docs-reference-command-line-tools-reference-kubelet-tls-bootstrapping-check">
<label for="m-docs-reference-command-line-tools-reference-kubelet-tls-bootstrapping-check"><a href="https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet-tls-bootstrapping/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-command-line-tools-reference-kubelet-tls-bootstrapping"><span>TLS bootstrapping</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-docs-reference-config-api-li">
<input type="checkbox" id="m-docs-reference-config-api-check">
<label for="m-docs-reference-config-api-check"><a href="https://kubernetes.io/docs/reference/config-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-docs-reference-config-api"><span>Configuration APIs</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-client-authentication-v1-li">
<input type="checkbox" id="m-docs-reference-config-api-client-authentication-v1-check">
<label for="m-docs-reference-config-api-client-authentication-v1-check"><a href="https://kubernetes.io/docs/reference/config-api/client-authentication.v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-client-authentication-v1"><span>Client Authentication (v1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-client-authentication-v1beta1-li">
<input type="checkbox" id="m-docs-reference-config-api-client-authentication-v1beta1-check">
<label for="m-docs-reference-config-api-client-authentication-v1beta1-check"><a href="https://kubernetes.io/docs/reference/config-api/client-authentication.v1beta1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-client-authentication-v1beta1"><span>Client Authentication (v1beta1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-apiserver-audit-v1-li">
<input type="checkbox" id="m-docs-reference-config-api-apiserver-audit-v1-check">
<label for="m-docs-reference-config-api-apiserver-audit-v1-check"><a href="https://kubernetes.io/docs/reference/config-api/apiserver-audit.v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-apiserver-audit-v1"><span>kube-apiserver Audit Configuration (v1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-apiserver-config-v1-li">
<input type="checkbox" id="m-docs-reference-config-api-apiserver-config-v1-check">
<label for="m-docs-reference-config-api-apiserver-config-v1-check"><a href="https://kubernetes.io/docs/reference/config-api/apiserver-config.v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-apiserver-config-v1"><span>kube-apiserver Configuration (v1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-apiserver-config-v1alpha1-li">
<input type="checkbox" id="m-docs-reference-config-api-apiserver-config-v1alpha1-check">
<label for="m-docs-reference-config-api-apiserver-config-v1alpha1-check"><a href="https://kubernetes.io/docs/reference/config-api/apiserver-config.v1alpha1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-apiserver-config-v1alpha1"><span>kube-apiserver Configuration (v1alpha1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-apiserver-encryption-v1-li">
<input type="checkbox" id="m-docs-reference-config-api-apiserver-encryption-v1-check">
<label for="m-docs-reference-config-api-apiserver-encryption-v1-check"><a href="https://kubernetes.io/docs/reference/config-api/apiserver-encryption.v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-apiserver-encryption-v1"><span>kube-apiserver Encryption Configuration (v1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kube-proxy-config-v1alpha1-li">
<input type="checkbox" id="m-docs-reference-config-api-kube-proxy-config-v1alpha1-check">
<label for="m-docs-reference-config-api-kube-proxy-config-v1alpha1-check"><a href="https://kubernetes.io/docs/reference/config-api/kube-proxy-config.v1alpha1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kube-proxy-config-v1alpha1"><span>kube-proxy Configuration (v1alpha1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kube-scheduler-config-v1beta2-li">
<input type="checkbox" id="m-docs-reference-config-api-kube-scheduler-config-v1beta2-check">
<label for="m-docs-reference-config-api-kube-scheduler-config-v1beta2-check"><a href="https://kubernetes.io/docs/reference/config-api/kube-scheduler-config.v1beta2/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kube-scheduler-config-v1beta2"><span>kube-scheduler Configuration (v1beta2)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kube-scheduler-config-v1beta3-li">
<input type="checkbox" id="m-docs-reference-config-api-kube-scheduler-config-v1beta3-check">
<label for="m-docs-reference-config-api-kube-scheduler-config-v1beta3-check"><a href="https://kubernetes.io/docs/reference/config-api/kube-scheduler-config.v1beta3/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kube-scheduler-config-v1beta3"><span>kube-scheduler Configuration (v1beta3)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kubeadm-config-v1beta2-li">
<input type="checkbox" id="m-docs-reference-config-api-kubeadm-config-v1beta2-check">
<label for="m-docs-reference-config-api-kubeadm-config-v1beta2-check"><a href="https://kubernetes.io/docs/reference/config-api/kubeadm-config.v1beta2/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kubeadm-config-v1beta2"><span>kubeadm Configuration (v1beta2)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kubeadm-config-v1beta3-li">
<input type="checkbox" id="m-docs-reference-config-api-kubeadm-config-v1beta3-check">
<label for="m-docs-reference-config-api-kubeadm-config-v1beta3-check"><a href="https://kubernetes.io/docs/reference/config-api/kubeadm-config.v1beta3/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kubeadm-config-v1beta3"><span>kubeadm Configuration (v1beta3)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kubelet-config-v1alpha1-li">
<input type="checkbox" id="m-docs-reference-config-api-kubelet-config-v1alpha1-check">
<label for="m-docs-reference-config-api-kubelet-config-v1alpha1-check"><a href="https://kubernetes.io/docs/reference/config-api/kubelet-config.v1alpha1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kubelet-config-v1alpha1"><span>Kubelet Configuration (v1alpha1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kubelet-config-v1beta1-li">
<input type="checkbox" id="m-docs-reference-config-api-kubelet-config-v1beta1-check">
<label for="m-docs-reference-config-api-kubelet-config-v1beta1-check"><a href="https://kubernetes.io/docs/reference/config-api/kubelet-config.v1beta1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kubelet-config-v1beta1"><span>Kubelet Configuration (v1beta1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kubelet-credentialprovider-v1alpha1-li">
<input type="checkbox" id="m-docs-reference-config-api-kubelet-credentialprovider-v1alpha1-check">
<label for="m-docs-reference-config-api-kubelet-credentialprovider-v1alpha1-check"><a href="https://kubernetes.io/docs/reference/config-api/kubelet-credentialprovider.v1alpha1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kubelet-credentialprovider-v1alpha1"><span>Kubelet CredentialProvider (v1alpha1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-kubelet-credentialprovider-v1beta1-li">
<input type="checkbox" id="m-docs-reference-config-api-kubelet-credentialprovider-v1beta1-check">
<label for="m-docs-reference-config-api-kubelet-credentialprovider-v1beta1-check"><a href="https://kubernetes.io/docs/reference/config-api/kubelet-credentialprovider.v1beta1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-kubelet-credentialprovider-v1beta1"><span>Kubelet CredentialProvider (v1beta1)</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-reference-config-api-apiserver-webhookadmission-v1-li">
<input type="checkbox" id="m-docs-reference-config-api-apiserver-webhookadmission-v1-check">
<label for="m-docs-reference-config-api-apiserver-webhookadmission-v1-check"><a href="https://kubernetes.io/docs/reference/config-api/apiserver-webhookadmission.v1/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-reference-config-api-apiserver-webhookadmission-v1"><span>WebhookAdmission Configuration (v1)</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-reference-scheduling-li">
<input type="checkbox" id="m-ko-docs-reference-scheduling-check">
<label for="m-ko-docs-reference-scheduling-check"><a href="https://kubernetes.io/ko/docs/reference/scheduling/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-scheduling"><span>스케줄링</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-scheduling-config-li">
<input type="checkbox" id="m-ko-docs-reference-scheduling-config-check">
<label for="m-ko-docs-reference-scheduling-config-check"><a href="https://kubernetes.io/ko/docs/reference/scheduling/config/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-scheduling-config"><span>스케줄러 구성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-scheduling-policies-li">
<input type="checkbox" id="m-ko-docs-reference-scheduling-policies-check">
<label for="m-ko-docs-reference-scheduling-policies-check"><a href="https://kubernetes.io/ko/docs/reference/scheduling/policies/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-reference-scheduling-policies"><span>스케줄링 정책</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-reference-tools-li">
<input type="checkbox" id="m-ko-docs-reference-tools-check">
<label for="m-ko-docs-reference-tools-check"><a href="https://kubernetes.io/ko/docs/reference/tools/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-reference-tools"><span>도구</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-contribute-li">
<input type="checkbox" id="m-ko-docs-contribute-check">
<label for="m-ko-docs-contribute-check"><a href="https://kubernetes.io/ko/docs/contribute/" title="K8s 문서에 기여하기" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-contribute"><span>기여</span></a></label>
<ul class="ul-2 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-suggest-improvements-li">
<input type="checkbox" id="m-ko-docs-contribute-suggest-improvements-check">
<label for="m-ko-docs-contribute-suggest-improvements-check"><a href="https://kubernetes.io/ko/docs/contribute/suggest-improvements/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-suggest-improvements"><span>콘텐츠 개선 제안</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-contribute-new-content-li">
<input type="checkbox" id="m-ko-docs-contribute-new-content-check">
<label for="m-ko-docs-contribute-new-content-check"><a href="https://kubernetes.io/ko/docs/contribute/new-content/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-contribute-new-content"><span>새로운 콘텐츠 기여하기</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-new-content-open-a-pr-li">
<input type="checkbox" id="m-ko-docs-contribute-new-content-open-a-pr-check">
<label for="m-ko-docs-contribute-new-content-open-a-pr-check"><a href="https://kubernetes.io/ko/docs/contribute/new-content/open-a-pr/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-new-content-open-a-pr"><span>풀 리퀘스트 열기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-new-content-new-features-li">
<input type="checkbox" id="m-docs-contribute-new-content-new-features-check">
<label for="m-docs-contribute-new-content-new-features-check"><a href="https://kubernetes.io/docs/contribute/new-content/new-features/" title="Documenting a feature for a release" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-new-content-new-features"><span>Documenting for a release</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-new-content-blogs-case-studies-li">
<input type="checkbox" id="m-docs-contribute-new-content-blogs-case-studies-check">
<label for="m-docs-contribute-new-content-blogs-case-studies-check"><a href="https://kubernetes.io/docs/contribute/new-content/blogs-case-studies/" title="Submitting blog posts and case studies" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-new-content-blogs-case-studies"><span>Blogs and case studies</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-contribute-review-li">
<input type="checkbox" id="m-ko-docs-contribute-review-check">
<label for="m-ko-docs-contribute-review-check"><a href="https://kubernetes.io/ko/docs/contribute/review/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-contribute-review"><span>변경 사항 리뷰하기</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-review-reviewing-prs-li">
<input type="checkbox" id="m-ko-docs-contribute-review-reviewing-prs-check">
<label for="m-ko-docs-contribute-review-reviewing-prs-check"><a href="https://kubernetes.io/ko/docs/contribute/review/reviewing-prs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-review-reviewing-prs"><span>풀 리퀘스트 리뷰</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-review-for-approvers-li">
<input type="checkbox" id="m-ko-docs-contribute-review-for-approvers-check">
<label for="m-ko-docs-contribute-review-for-approvers-check"><a href="https://kubernetes.io/ko/docs/contribute/review/for-approvers/" title="승인자와 리뷰어의 리뷰" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-review-for-approvers"><span>승인자와 리뷰어용</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-localization-li">
<input type="checkbox" id="m-docs-contribute-localization-check">
<label for="m-docs-contribute-localization-check"><a href="https://kubernetes.io/docs/contribute/localization/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-localization"><span>Localizing Kubernetes documentation</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-contribute-participate-li">
<input type="checkbox" id="m-ko-docs-contribute-participate-check">
<label for="m-ko-docs-contribute-participate-check"><a href="https://kubernetes.io/ko/docs/contribute/participate/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-contribute-participate"><span>SIG Docs에 참여하기</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-participate-roles-and-responsibilities-li">
<input type="checkbox" id="m-ko-docs-contribute-participate-roles-and-responsibilities-check">
<label for="m-ko-docs-contribute-participate-roles-and-responsibilities-check"><a href="https://kubernetes.io/ko/docs/contribute/participate/roles-and-responsibilities/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-participate-roles-and-responsibilities"><span>역할과 책임</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-participate-pr-wranglers-li">
<input type="checkbox" id="m-ko-docs-contribute-participate-pr-wranglers-check">
<label for="m-ko-docs-contribute-participate-pr-wranglers-check"><a href="https://kubernetes.io/ko/docs/contribute/participate/pr-wranglers/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-participate-pr-wranglers"><span>PR 랭글러(PR Wrangler)</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-contribute-generate-ref-docs-li">
<input type="checkbox" id="m-ko-docs-contribute-generate-ref-docs-check">
<label for="m-ko-docs-contribute-generate-ref-docs-check"><a href="https://kubernetes.io/ko/docs/contribute/generate-ref-docs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-contribute-generate-ref-docs"><span>레퍼런스 문서 개요</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-generate-ref-docs-contribute-upstream-li">
<input type="checkbox" id="m-docs-contribute-generate-ref-docs-contribute-upstream-check">
<label for="m-docs-contribute-generate-ref-docs-contribute-upstream-check"><a href="https://kubernetes.io/docs/contribute/generate-ref-docs/contribute-upstream/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-generate-ref-docs-contribute-upstream"><span>Contributing to the Upstream Kubernetes Code</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-generate-ref-docs-quickstart-li">
<input type="checkbox" id="m-ko-docs-contribute-generate-ref-docs-quickstart-check">
<label for="m-ko-docs-contribute-generate-ref-docs-quickstart-check"><a href="https://kubernetes.io/ko/docs/contribute/generate-ref-docs/quickstart/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-generate-ref-docs-quickstart"><span>퀵스타트 가이드</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-generate-ref-docs-kubernetes-api-li">
<input type="checkbox" id="m-docs-contribute-generate-ref-docs-kubernetes-api-check">
<label for="m-docs-contribute-generate-ref-docs-kubernetes-api-check"><a href="https://kubernetes.io/docs/contribute/generate-ref-docs/kubernetes-api/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-generate-ref-docs-kubernetes-api"><span>Generating Reference Documentation for the Kubernetes API</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-generate-ref-docs-kubectl-li">
<input type="checkbox" id="m-docs-contribute-generate-ref-docs-kubectl-check">
<label for="m-docs-contribute-generate-ref-docs-kubectl-check"><a href="https://kubernetes.io/docs/contribute/generate-ref-docs/kubectl/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-generate-ref-docs-kubectl"><span>Generating Reference Documentation for kubectl Commands</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-generate-ref-docs-kubernetes-components-li">
<input type="checkbox" id="m-docs-contribute-generate-ref-docs-kubernetes-components-check">
<label for="m-docs-contribute-generate-ref-docs-kubernetes-components-check"><a href="https://kubernetes.io/docs/contribute/generate-ref-docs/kubernetes-components/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-generate-ref-docs-kubernetes-components"><span>Generating Reference Pages for Kubernetes Components and Tools</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-generate-ref-docs-prerequisites-ref-docs-li">
<input type="checkbox" id="m-ko-docs-contribute-generate-ref-docs-prerequisites-ref-docs-check">
<label for="m-ko-docs-contribute-generate-ref-docs-prerequisites-ref-docs-check"><a href="https://kubernetes.io/ko/docs/contribute/generate-ref-docs/prerequisites-ref-docs/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-generate-ref-docs-prerequisites-ref-docs"><span></span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section with-child" id="m-ko-docs-contribute-style-li">
<input type="checkbox" id="m-ko-docs-contribute-style-check">
<label for="m-ko-docs-contribute-style-check"><a href="https://kubernetes.io/ko/docs/contribute/style/" class="align-left pl-0 td-sidebar-link td-sidebar-link__section" id="m-ko-docs-contribute-style"><span>문서 스타일 개요</span></a></label>
<ul class="ul-3 foldable">
<li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-style-content-guide-li">
<input type="checkbox" id="m-docs-contribute-style-content-guide-check">
<label for="m-docs-contribute-style-content-guide-check"><a href="https://kubernetes.io/docs/contribute/style/content-guide/" title="Documentation Content Guide" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-style-content-guide"><span>Content guide</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-style-style-guide-li">
<input type="checkbox" id="m-docs-contribute-style-style-guide-check">
<label for="m-docs-contribute-style-style-guide-check"><a href="https://kubernetes.io/docs/contribute/style/style-guide/" title="Documentation Style Guide" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-style-style-guide"><span>Style guide</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-style-diagram-guide-li">
<input type="checkbox" id="m-docs-contribute-style-diagram-guide-check">
<label for="m-docs-contribute-style-diagram-guide-check"><a href="https://kubernetes.io/docs/contribute/style/diagram-guide/" title="Diagram Guide" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-style-diagram-guide"><span>Diagram guide</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-style-write-new-topic-li">
<input type="checkbox" id="m-ko-docs-contribute-style-write-new-topic-check">
<label for="m-ko-docs-contribute-style-write-new-topic-check"><a href="https://kubernetes.io/ko/docs/contribute/style/write-new-topic/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-style-write-new-topic"><span>새로운 주제의 문서 작성</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-style-page-content-types-li">
<input type="checkbox" id="m-docs-contribute-style-page-content-types-check">
<label for="m-docs-contribute-style-page-content-types-check"><a href="https://kubernetes.io/docs/contribute/style/page-content-types/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-style-page-content-types"><span>Page content types</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-style-content-organization-li">
<input type="checkbox" id="m-docs-contribute-style-content-organization-check">
<label for="m-docs-contribute-style-content-organization-check"><a href="https://kubernetes.io/docs/contribute/style/content-organization/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-style-content-organization"><span>Content organization</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-docs-contribute-style-hugo-shortcodes-li">
<input type="checkbox" id="m-docs-contribute-style-hugo-shortcodes-check">
<label for="m-docs-contribute-style-hugo-shortcodes-check"><a href="https://kubernetes.io/docs/contribute/style/hugo-shortcodes/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-docs-contribute-style-hugo-shortcodes"><span>Custom Hugo Shortcodes</span></a></label>
</li>
</ul>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-advanced-li">
<input type="checkbox" id="m-ko-docs-contribute-advanced-check">
<label for="m-ko-docs-contribute-advanced-check"><a href="https://kubernetes.io/ko/docs/contribute/advanced/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-advanced"><span>고급 기여</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-analytics-li">
<input type="checkbox" id="m-ko-docs-contribute-analytics-check">
<label for="m-ko-docs-contribute-analytics-check"><a href="https://kubernetes.io/ko/docs/contribute/analytics/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-analytics"><span>사이트 분석 보기</span></a></label>
</li><li class="td-sidebar-nav__section-title td-sidebar-nav__section without-child" id="m-ko-docs-contribute-localization-ko-li">
<input type="checkbox" id="m-ko-docs-contribute-localization-ko-check">
<label for="m-ko-docs-contribute-localization-ko-check"><a href="https://kubernetes.io/ko/docs/contribute/localization_ko/" class="align-left pl-0 td-sidebar-link td-sidebar-link__page" id="m-ko-docs-contribute-localization-ko"><span>쿠버네티스 문서 한글화 가이드</span></a></label>
</li>
</ul>
</li><a class="td-sidebar-link td-sidebar-link__page" id="m-docs-test" target="_blank" href="https://kubernetes.io/docs/test/">
Docs smoke test page <small>(EN)</small></a>
</ul>
</li>
</ul>
</nav>
</div>
</div>
<main class="col-12 col-md-9 col-xl-8 pl-md-5" role="main">
<nav aria-label="breadcrumb" class="td-breadcrumbs">
<ol class="breadcrumb">
<li class="breadcrumb-item">
<a href="https://kubernetes.io/ko/docs/">쿠버네티스 문서</a>
</li>
<li class="breadcrumb-item">
<a href="https://kubernetes.io/ko/docs/concepts/">개념</a>
</li>
<li class="breadcrumb-item">
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/">서비스, 로드밸런싱, 네트워킹</a>
</li>
<li class="breadcrumb-item active" aria-current="page">
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/">인그레스(Ingress)</a>
</li>
</ol>
</nav>
<div class="td-content">
<h1>인그레스(Ingress)</h1>
<p>
</p><div style="margin-top:10px;margin-bottom:10px">
<b>FEATURE STATE:</b> <code>Kubernetes v1.19 [stable]</code>
</div>
<p>클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리함.</p>
<p>인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공할 수 있다.</p><p></p>
<h2 id="용어">용어<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9A%A9%EC%96%B4" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>이 가이드는 용어의 명확성을 위해 다음과 같이 정의한다.</p>
<ul>
<li>노드(Node): 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신.</li>
<li>클러스터(Cluster): 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합. 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아니다.</li>
<li>에지 라우터(Edge router): 클러스터에 방화벽 정책을 적용하는 라우터. 이것은 클라우드 공급자 또는 물리적 하드웨어의 일부에서 관리하는 게이트웨이일 수 있다.</li>
<li>클러스터 네트워크(Cluster network): 쿠버네티스 <a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/networking/">네트워킹 모델</a>에 따라 클러스터 내부에서 통신을 용이하게 하는 논리적 또는 물리적 링크 집합.</li>
<li>서비스: <a class="glossary-tooltip" title="" data-toggle="tooltip" data-placement="top" href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/labels" target="_blank" aria-label="레이블" data-original-title="사용자에게 의미 있고 관련성 높은 특징으로 식별할 수 있도록 오브젝트에 태그를 붙인다.">레이블</a> 셀렉터를 사용해서 파드 집합을 식별하는 쿠버네티스 <a class="glossary-tooltip" title="" data-toggle="tooltip" data-placement="top" href="https://kubernetes.io/ko/docs/concepts/services-networking/service/" target="_blank" aria-label="서비스" data-original-title="네트워크 서비스로 파드 집합에서 실행 중인 애플리케이션을 노출하는 방법">서비스</a>. 달리 언급하지 않으면 서비스는 클러스터 네트워크 내에서만 라우팅 가능한 가상 IP를 가지고 있다고 가정한다.</li>
</ul>
<h2 id="인그레스란">인그레스란?<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4%EB%9E%80" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p><a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#ingress-v1-networking-k8s-io">인그레스</a>는 클러스터 외부에서 클러스터 내부
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/" target="_blank">서비스</a>로 HTTP와 HTTPS 경로를 노출한다.
트래픽 라우팅은 인그레스 리소스에 정의된 규칙에 의해 컨트롤된다.</p>
<p>다음은 인그레스가 모든 트래픽을 하나의 서비스로 보내는 간단한 예시이다.
</p><figure>
<div class="mermaid" data-processed="true"><svg id="mermaid-1651936427813" width="100%" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="224" style="max-width: 696.25px;" viewBox="0 0 696.25 224"><style>#mermaid-1651936427813 {font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}#mermaid-1651936427813 .error-icon{fill:#552222;}#mermaid-1651936427813 .error-text{fill:#552222;stroke:#552222;}#mermaid-1651936427813 .edge-thickness-normal{stroke-width:2px;}#mermaid-1651936427813 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1651936427813 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1651936427813 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1651936427813 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1651936427813 .marker{fill:#333333;stroke:#333333;}#mermaid-1651936427813 .marker.cross{stroke:#333333;}#mermaid-1651936427813 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1651936427813 .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1651936427813 .cluster-label text{fill:#333;}#mermaid-1651936427813 .cluster-label span{color:#333;}#mermaid-1651936427813 .label text,#mermaid-1651936427813 span{fill:#333;color:#333;}#mermaid-1651936427813 .node rect,#mermaid-1651936427813 .node circle,#mermaid-1651936427813 .node ellipse,#mermaid-1651936427813 .node polygon,#mermaid-1651936427813 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1651936427813 .node .label{text-align:center;}#mermaid-1651936427813 .node.clickable{cursor:pointer;}#mermaid-1651936427813 .arrowheadPath{fill:#333333;}#mermaid-1651936427813 .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1651936427813 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1651936427813 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-1651936427813 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-1651936427813 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1651936427813 .cluster text{fill:#333;}#mermaid-1651936427813 .cluster span{color:#333;}#mermaid-1651936427813 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1651936427813 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-1651936427813 .plain&gt;*{fill:#ddd!important;stroke:#fff!important;stroke-width:4px!important;color:#000!important;}#mermaid-1651936427813 .plain span{fill:#ddd!important;stroke:#fff!important;stroke-width:4px!important;color:#000!important;}#mermaid-1651936427813 .k8s&gt;*{fill:#326ce5!important;stroke:#fff!important;stroke-width:4px!important;color:#fff!important;}#mermaid-1651936427813 .k8s span{fill:#326ce5!important;stroke:#fff!important;stroke-width:4px!important;color:#fff!important;}#mermaid-1651936427813 .cluster&gt;*{fill:#fff!important;stroke:#bbb!important;stroke-width:2px!important;color:#326ce5!important;}#mermaid-1651936427813 .cluster span{fill:#fff!important;stroke:#bbb!important;stroke-width:2px!important;color:#326ce5!important;}</style><g><g class="output"><g class="clusters"><g class="cluster" id="flowchart-클러스터-18" transform="translate(483.9375,112)" style="opacity: 1;"><rect width="408.625" height="208" x="-204.3125" y="-104"></rect><g class="label" transform="translate(0, -90)" id="mermaid-1651936427813Text"><g transform="translate(-27.6875,-12)"><foreignobject width="55.375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">클러스터</div></foreignobject></g></g></g></g><g class="edgePaths"><g class="edgePath LS-client LE-ingress" id="L-client-ingress" style="opacity: 1;"><path class="path" d="M108.203125,112L122.48828125,112C136.7734375,112,165.34375,112,193.9140625,112C222.484375,112,251.0546875,112,269.5065104166667,112C287.9583333333333,112,296.2916666666667,112,300.4583333333333,112L304.625,112" marker-end="url(#arrowhead41)" style="fill:none;stroke-width:2px;stroke-dasharray:3;"></path><defs><marker id="arrowhead41" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-ingress LE-service" id="L-ingress-service" style="opacity: 1;"><path class="path" d="M380,112L390.3359375,112C400.671875,112,421.34375,112,442.015625,112C462.6875,112,483.359375,112,493.6953125,112L504.03125,112" marker-end="url(#arrowhead42)" style="fill:none"></path><defs><marker id="arrowhead42" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service LE-pod1" id="L-service-pod1" style="opacity: 1;"><path class="path" d="M560.8999335106383,90L565.8436945921986,85.83333333333333C570.7874556737589,81.66666666666667,580.6749778368794,73.33333333333333,589.7854055851063,69.16666666666667C598.8958333333334,65,607.2291666666666,65,611.3958333333334,65L615.5625,65" marker-end="url(#arrowhead43)" style="fill:none"></path><defs><marker id="arrowhead43" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service LE-pod2" id="L-service-pod2" style="opacity: 1;"><path class="path" d="M560.8999335106383,134L565.8436945921986,138.16666666666666C570.7874556737589,142.33333333333334,580.6749778368794,150.66666666666666,589.7854055851063,154.83333333333334C598.8958333333334,159,607.2291666666666,159,611.3958333333334,159L615.5625,159" marker-end="url(#arrowhead44)" style="fill:none"></path><defs><marker id="arrowhead44" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(193.9140625,112)" style="opacity: 1;"><g transform="translate(-60.7109375,-24)" class="label"><rect rx="0" ry="0" width="121.421875" height="48"></rect><foreignobject width="121.421875" height="48"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-client-ingress" class="edgeLabel L-LS-client&#39; L-LE-ingress">인그레스-매니지드 <br> 로드 밸런서</span></div></foreignobject></g></g><g class="edgeLabel" transform="translate(442.015625,112)" style="opacity: 1;"><g transform="translate(-37.015625,-12)" class="label"><rect rx="0" ry="0" width="74.03125" height="24"></rect><foreignobject width="74.03125" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-ingress-service" class="edgeLabel L-LS-ingress&#39; L-LE-service">라우팅 규칙</span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service-pod1" class="edgeLabel L-LS-service&#39; L-LE-pod1"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service-pod2" class="edgeLabel L-LS-service&#39; L-LE-pod2"></span></div></foreignobject></g></g></g><g class="nodes"><g class="node k8s" id="flowchart-ingress-10" transform="translate(342.3125,112)" style="opacity: 1;"><rect rx="0" ry="0" x="-37.6875" y="-22" width="75.375" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27.6875,-12)"><foreignobject width="55.375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">인그레스</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod1-15" transform="translate(639.40625,65)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-service-12" transform="translate(534.796875,112)" style="opacity: 1;"><rect rx="0" ry="0" x="-30.765625" y="-22" width="61.53125" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-20.765625,-12)"><foreignobject width="41.53125" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">서비스</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod2-17" transform="translate(639.40625,159)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node plain" id="flowchart-client-9" transform="translate(58.1015625,112)" style="opacity: 1;"><rect rx="22" ry="22" x="-50.1015625" y="-22" width="100.203125" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-34.6015625,-12)"><foreignobject width="69.203125" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">클라이언트</div></foreignobject></g></g></g></g></g></g></svg></div>
</figure>
<noscript>
<div class="alert alert-secondary callout" role=alert>
<em class=javascript-required>JavaScript must be <a href=https://www.enable-javascript.com/>enabled</a> to view this content</em>
</div>
</noscript><p></p>
<p>인그레스는 외부에서 서비스로 접속이 가능한 URL, 로드 밸런스 트래픽, SSL / TLS 종료 그리고 이름-기반의 가상 호스팅을 제공하도록 구성할 수 있다. <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers">인그레스 컨트롤러</a>는 일반적으로 로드 밸런서를 사용해서 인그레스를 수행할 책임이 있으며, 트래픽을 처리하는데 도움이 되도록 에지 라우터 또는 추가 프런트 엔드를 구성할 수도 있다.</p>
<p>인그레스는 임의의 포트 또는 프로토콜을 노출시키지 않는다. HTTP와 HTTPS 이외의 서비스를 인터넷에 노출하려면 보통
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/#type-nodeport">Service.Type=NodePort</a> 또는
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/#loadbalancer">Service.Type=LoadBalancer</a> 유형의 서비스를 사용한다.</p>
<h2 id="전제-조건들">전제 조건들<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%A0%84%EC%A0%9C-%EC%A1%B0%EA%B1%B4%EB%93%A4" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers">인그레스 컨트롤러</a>가 있어야 인그레스를 충족할 수 있다. 인그레스 리소스만 생성한다면 효과가 없다.</p>
<p><a href="https://kubernetes.github.io/ingress-nginx/deploy/">ingress-nginx</a>와 같은 인그레스 컨트롤러를 배포해야 할 수도 있다. 여러
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers">인그레스 컨트롤러</a> 중에서 선택할 수도 있다.</p>
<p>이상적으로, 모든 인그레스 컨트롤러는 참조 사양이 맞아야 한다. 실제로, 다양한 인그레스
컨트롤러는 조금 다르게 작동한다.</p>
<div class="alert alert-info note callout" role="alert">
<strong>참고:</strong> 인그레스 컨트롤러의 설명서를 검토하여 선택 시 주의 사항을 이해해야 한다.
</div>
<h2 id="인그레스-리소스">인그레스 리소스<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EB%A6%AC%EC%86%8C%EC%8A%A4" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>최소한의 인그레스 리소스 예제:</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/minimal-ingress.yaml" download="service/networking/minimal-ingress.yaml"><code>service/networking/minimal-ingress.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-minimal-ingress-yaml&#39;)" title="Copy service/networking/minimal-ingress.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-minimal-ingress-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>minimal-ingress<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">annotations</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">nginx.ingress.kubernetes.io/rewrite-target</span>:<span style="color:#bbb"> </span>/<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">ingressClassName</span>:<span style="color:#bbb"> </span>nginx-example<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/testpath<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>test<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<p>다른 모든 쿠버네티스 리소스와 마찬가지로 인그레스에는 <code>apiVersion</code>, <code>kind</code>, 그리고 <code>metadata</code> 필드가 필요하다.
인그레스 오브젝트의 이름은 유효한
<a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/names/#dns-%EC%84%9C%EB%B8%8C%EB%8F%84%EB%A9%94%EC%9D%B8-%EC%9D%B4%EB%A6%84">DNS 서브도메인 이름</a>이어야 한다.
설정 파일의 작성에 대한 일반적인 내용은 <a href="https://kubernetes.io/ko/docs/tasks/run-application/run-stateless-application-deployment/">애플리케이션 배포하기</a>, <a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/">컨테이너 구성하기</a>, <a href="https://kubernetes.io/ko/docs/concepts/cluster-administration/manage-deployment/">리소스 관리하기</a>를 참조한다.
인그레스는 종종 어노테이션을 이용해서 인그레스 컨트롤러에 따라 몇 가지 옵션을 구성하는데,
그 예시는 <a href="https://github.com/kubernetes/ingress-nginx/blob/master/docs/examples/rewrite/README.md">재작성-타겟 어노테이션</a>이다.
서로 다른 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers">인그레스 컨트롤러</a>는 서로 다른 어노테이션을 지원한다.
지원되는 어노테이션을 확인하려면 선택한 인그레스 컨트롤러의 설명서를 검토한다.</p>
<p>인그레스 <a href="https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status">사양</a>
에는 로드 밸런서 또는 프록시 서버를 구성하는데 필요한 모든 정보가 있다. 가장 중요한 것은,
들어오는 요청과 일치하는 규칙 목록을 포함하는 것이다. 인그레스 리소스는 HTTP(S) 트래픽을
지시하는 규칙만 지원한다.</p>
<p><code>ingressClassName</code>을 생략하려면, <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#default-ingress-class">기본 인그레스 클래스</a>가
정의되어 있어야 한다.</p>
<p>몇몇 인그레스 컨트롤러는 기본 <code>IngressClass</code>가 정의되어 있지 않아도 동작한다.
예를 들어, Ingress-NGINX 컨트롤러는 <code>--watch-ingress-without-class</code>
<a href="https://kubernetes.github.io/ingress-nginx/#what-is-the-flag-watch-ingress-without-class">플래그</a>를 이용하여 구성될 수 있다.
하지만 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#default-ingress-class">아래</a>에 나와 있는 것과 같이 기본 <code>IngressClass</code>를 명시하는 것을
<a href="https://kubernetes.github.io/ingress-nginx/#i-have-only-one-instance-of-the-ingresss-nginx-controller-in-my-cluster-what-should-i-do">권장</a>한다.</p>
<h3 id="인그레스-규칙">인그레스 규칙<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EA%B7%9C%EC%B9%99" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>각 HTTP 규칙에는 다음의 정보가 포함된다.</p>
<ul>
<li>선택적 호스트. 이 예시에서는, 호스트가 지정되지 않기에 지정된 IP 주소를 통해 모든 인바운드
HTTP 트래픽에 규칙이 적용 된다. 만약 호스트가 제공되면(예,
foo.bar.com), 규칙이 해당 호스트에 적용된다.</li>
<li>경로 목록 (예, <code>/testpath</code>)에는 각각 <code>service.name</code> 과
<code>service.port.name</code> 또는 <code>service.port.number</code> 가 정의되어 있는 관련
백엔드를 가지고 있다. 로드 밸런서가 트래픽을 참조된 서비스로
보내기 전에 호스트와 경로가 모두 수신 요청의 내용과
일치해야 한다.</li>
<li>백엔드는 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/">서비스 문서</a> 또는 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#resource-backend">사용자 정의 리소스 백엔드</a>에 설명된 바와 같이
서비스와 포트 이름의 조합이다. 호스트와 규칙 경로가 일치하는 인그레스에 대한
HTTP(와 HTTPS) 요청은 백엔드 목록으로 전송된다.</li>
</ul>
<p><code>defaultBackend</code> 는 종종 사양의 경로와 일치하지 않는 서비스에 대한 모든 요청을 처리하도록 인그레스
컨트롤러에 구성되는 경우가 많다.</p>
<h3 id="default-backend">DefaultBackend<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#default-backend" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>규칙이 없는 인그레스는 모든 트래픽을 단일 기본 백엔드로 전송한다. <code>defaultBackend</code> 는 일반적으로
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers">인그레스 컨트롤러</a>의 구성 옵션이며, 인그레스 리소스에 지정되어 있지 않다.</p>
<p>만약 인그레스 오브젝트의 HTTP 요청과 일치하는 호스트 또는 경로가 없으면, 트래픽은
기본 백엔드로 라우팅 된다.</p>
<h3 id="resource-backend">리소스 백엔드<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#resource-backend" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p><code>Resource</code> 백엔드는 인그레스 오브젝트와 동일한 네임스페이스 내에 있는
다른 쿠버네티스 리소스에 대한 ObjectRef이다. <code>Resource</code> 는 서비스와
상호 배타적인 설정이며, 둘 다 지정하면 유효성 검사에 실패한다. <code>Resource</code>
백엔드의 일반적인 용도는 정적 자산이 있는 오브젝트 스토리지 백엔드로 데이터를
수신하는 것이다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/ingress-resource-backend.yaml" download="service/networking/ingress-resource-backend.yaml"><code>service/networking/ingress-resource-backend.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-ingress-resource-backend-yaml&#39;)" title="Copy service/networking/ingress-resource-backend.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-ingress-resource-backend-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>ingress-resource-backend<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">defaultBackend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">resource</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">apiGroup</span>:<span style="color:#bbb"> </span>k8s.example.com<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>StorageBucket<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>static-assets<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span>- <span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span>- <span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/icons<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>ImplementationSpecific<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">resource</span>:<span style="color:#bbb">
</span><span style="color:#bbb">                </span><span style="color:green;font-weight:700">apiGroup</span>:<span style="color:#bbb"> </span>k8s.example.com<span style="color:#bbb">
</span><span style="color:#bbb">                </span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>StorageBucket<span style="color:#bbb">
</span><span style="color:#bbb">                </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>icon-assets<span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<p>위의 인그레스를 생성한 후, 다음의 명령으로 확인할 수 있다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-bash" data-lang="bash">kubectl describe ingress ingress-resource-backend
</code></pre></div><pre><code>Name:             ingress-resource-backend
Namespace:        default
Address:
Default backend:  APIGroup: k8s.example.com, Kind: StorageBucket, Name: static-assets
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /icons   APIGroup: k8s.example.com, Kind: StorageBucket, Name: icon-assets
Annotations:  &lt;none&gt;
Events:       &lt;none&gt;
</code></pre><h3 id="경로-유형">경로 유형<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EA%B2%BD%EB%A1%9C-%EC%9C%A0%ED%98%95" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>인그레스의 각 경로에는 해당 경로 유형이 있어야 한다. 명시적
<code>pathType</code> 을 포함하지 않는 경로는 유효성 검사에 실패한다. 지원되는
경로 유형은 세 가지이다.</p>
<ul>
<li>
<p><code>ImplementationSpecific</code>: 이 경로 유형의 일치 여부는 IngressClass에 따라
달라진다. 이를 구현할 때 별도 <code>pathType</code> 으로 처리하거나, <code>Prefix</code> 또는 <code>Exact</code>
경로 유형과 같이 동일하게 처리할 수 있다.</p>
</li>
<li>
<p><code>Exact</code>: URL 경로의 대소문자를 엄격하게 일치시킨다.</p>
</li>
<li>
<p><code>Prefix</code>: URL 경로의 접두사를 <code>/</code> 를 기준으로 분리한 값과 일치시킨다.
일치는 대소문자를 구분하고,
요소별로 경로 요소에 대해 수행한다.
모든 <em>p</em> 가 요청 경로의 요소별 접두사가 <em>p</em> 인 경우
요청은 <em>p</em> 경로에 일치한다.</p>
<div class="alert alert-info note callout" role="alert">
<strong>참고:</strong> 경로의 마지막 요소가 요청 경로에 있는 마지막
요소의 하위 문자열인 경우에는 일치하지 않는다(예시: <code>/foo/bar</code> 는
<code>/foo/bar/baz</code> 와 일치하지만, <code>/foo/barbaz</code> 와는 일치하지 않는다).
</div>
</li>
</ul>
<h3 id="예제">예제<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%98%88%EC%A0%9C" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>경로</th>
<th>요청 경로</th>
<th>일치 여부</th>
</tr>
</thead>
<tbody>
<tr>
<td>Prefix</td>
<td><code>/</code></td>
<td>(모든 경로)</td>
<td>예</td>
</tr>
<tr>
<td>Exact</td>
<td><code>/foo</code></td>
<td><code>/foo</code></td>
<td>예</td>
</tr>
<tr>
<td>Exact</td>
<td><code>/foo</code></td>
<td><code>/bar</code></td>
<td>아니오</td>
</tr>
<tr>
<td>Exact</td>
<td><code>/foo</code></td>
<td><code>/foo/</code></td>
<td>아니오</td>
</tr>
<tr>
<td>Exact</td>
<td><code>/foo/</code></td>
<td><code>/foo</code></td>
<td>아니오</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/foo</code></td>
<td><code>/foo</code>, <code>/foo/</code></td>
<td>예</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/foo/</code></td>
<td><code>/foo</code>, <code>/foo/</code></td>
<td>예</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa/bb</code></td>
<td><code>/aaa/bbb</code></td>
<td>아니오</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa/bbb</code></td>
<td><code>/aaa/bbb</code></td>
<td>예</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa/bbb/</code></td>
<td><code>/aaa/bbb</code></td>
<td>예, 마지막 슬래시 무시함</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa/bbb</code></td>
<td><code>/aaa/bbb/</code></td>
<td>예, 마지막 슬래시 일치함</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa/bbb</code></td>
<td><code>/aaa/bbb/ccc</code></td>
<td>예, 하위 경로 일치함</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa/bbb</code></td>
<td><code>/aaa/bbbxyz</code></td>
<td>아니오, 문자열 접두사 일치하지 않음</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/</code>, <code>/aaa</code></td>
<td><code>/aaa/ccc</code></td>
<td>예, <code>/aaa</code> 접두사 일치함</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/</code>, <code>/aaa</code>, <code>/aaa/bbb</code></td>
<td><code>/aaa/bbb</code></td>
<td>예, <code>/aaa/bbb</code> 접두사 일치함</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/</code>, <code>/aaa</code>, <code>/aaa/bbb</code></td>
<td><code>/ccc</code></td>
<td>예, <code>/</code> 접두사 일치함</td>
</tr>
<tr>
<td>Prefix</td>
<td><code>/aaa</code></td>
<td><code>/ccc</code></td>
<td>아니오, 기본 백엔드 사용함</td>
</tr>
<tr>
<td>Mixed</td>
<td><code>/foo</code> (Prefix), <code>/foo</code> (Exact)</td>
<td><code>/foo</code></td>
<td>예, Exact 선호함</td>
</tr>
</tbody>
</table>
<h4 id="다중-일치">다중 일치<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EB%8B%A4%EC%A4%91-%EC%9D%BC%EC%B9%98" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h4>
<p>경우에 따라 인그레스의 여러 경로가 요청과 일치할 수 있다.
이 경우 가장 긴 일치하는 경로가 우선하게 된다. 두 개의 경로가
여전히 동일하게 일치하는 경우 접두사(prefix) 경로 유형보다
정확한(exact) 경로 유형을 가진 경로가 사용 된다.</p>
<h2 id="호스트네임-와일드카드">호스트네임 와일드카드<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%ED%98%B8%EC%8A%A4%ED%8A%B8%EB%84%A4%EC%9E%84-%EC%99%80%EC%9D%BC%EB%93%9C%EC%B9%B4%EB%93%9C" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>호스트는 정확한 일치(예: "<code>foo.bar.com</code>") 또는 와일드카드(예:
"<code>* .foo.com</code>")일 수 있다. 정확한 일치를 위해서는 HTTP <code>host</code> 헤더가
<code>host</code> 필드와 일치해야 한다. 와일드카드 일치를 위해서는 HTTP <code>host</code> 헤더가
와일드카드 규칙의 접미사와 동일해야 한다.</p>
<table>
<thead>
<tr>
<th>호스트</th>
<th>호스트 헤더</th>
<th>일치 여부</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>*.foo.com</code></td>
<td><code>bar.foo.com</code></td>
<td>공유 접미사를 기반으로 일치함</td>
</tr>
<tr>
<td><code>*.foo.com</code></td>
<td><code>baz.bar.foo.com</code></td>
<td>일치하지 않음, 와일드카드는 단일 DNS 레이블만 포함함</td>
</tr>
<tr>
<td><code>*.foo.com</code></td>
<td><code>foo.com</code></td>
<td>일치하지 않음, 와일드카드는 단일 DNS 레이블만 포함함</td>
</tr>
</tbody>
</table>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/ingress-wildcard-host.yaml" download="service/networking/ingress-wildcard-host.yaml"><code>service/networking/ingress-wildcard-host.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-ingress-wildcard-host-yaml&#39;)" title="Copy service/networking/ingress-wildcard-host.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-ingress-wildcard-host-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>ingress-wildcard-host<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span><span style="color:#b44">"foo.bar.com"</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/bar"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service1<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span><span style="color:#b44">"*.foo.com"</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/foo"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service2<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<h2 id="인그레스-클래스">인그레스 클래스<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%ED%81%B4%EB%9E%98%EC%8A%A4" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>인그레스는 서로 다른 컨트롤러에 의해 구현될 수 있으며, 종종 다른 구성으로
구현될 수 있다. 각 인그레스에서는 클래스를 구현해야하는 컨트롤러
이름을 포함하여 추가 구성이 포함된 IngressClass
리소스에 대한 참조 클래스를 지정해야 한다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/external-lb.yaml" download="service/networking/external-lb.yaml"><code>service/networking/external-lb.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-external-lb-yaml&#39;)" title="Copy service/networking/external-lb.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-external-lb-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>IngressClass<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>external-lb<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">controller</span>:<span style="color:#bbb"> </span>example.com/ingress-controller<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">parameters</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">apiGroup</span>:<span style="color:#bbb"> </span>k8s.example.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>IngressParameters<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>external-lb<span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<p>인그레스클래스의 <code>.spec.parameters</code> 필드를 사용하여
해당 인그레스클래스와 연관있는 환경 설정을 제공하는 다른 리소스를 참조할 수 있다.</p>
<p>사용 가능한 파라미터의 상세한 타입은
인그레스클래스의 <code>.spec.parameters</code> 필드에 명시한 인그레스 컨트롤러의 종류에 따라 다르다.</p>
<h3 id="인그레스클래스-범위">인그레스클래스 범위<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%B2%94%EC%9C%84" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>인그레스 컨트롤러의 종류에 따라, 클러스터 범위로 설정한 파라미터의 사용이 가능할 수도 있고,
또는 한 네임스페이스에서만 사용 가능할 수도 있다.</p>
<ul class="nav nav-tabs" id="tabs-ingressclass-parameter-scope" role="tablist"><li class="nav-item"><a data-toggle="tab" class="nav-link active" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#tabs-ingressclass-parameter-scope-0" role="tab" aria-controls="tabs-ingressclass-parameter-scope-0" aria-selected="true">클러스터</a></li>
<li class="nav-item"><a data-toggle="tab" class="nav-link" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#tabs-ingressclass-parameter-scope-1" role="tab" aria-controls="tabs-ingressclass-parameter-scope-1">네임스페이스</a></li></ul>
<div class="tab-content" id="tabs-ingressclass-parameter-scope"><div id="tabs-ingressclass-parameter-scope-0" class="tab-pane show active" role="tabpanel" aria-labelledby="tabs-ingressclass-parameter-scope-0">
<p></p><p>인그레스클래스 파라미터의 기본 범위는 클러스터 범위이다.</p>
<p><code>.spec.parameters</code> 필드만 설정하고 <code>.spec.parameters.scope</code> 필드는 지정하지 않거나,
<code>.spec.parameters.scope</code> 필드를 <code>Cluster</code>로 지정하면,
인그레스클래스는 클러스터 범위의 리소스를 참조한다.
파라미터의 <code>kind</code>(+<code>apiGroup</code>)는
클러스터 범위의 API (커스텀 리소스일 수도 있음) 를 참조하며,
파라미터의 <code>name</code>은
해당 API에 대한 특정 클러스터 범위 리소스를 가리킨다.</p>
<p>예시는 다음과 같다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:#00f;font-weight:700">---</span><span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>IngressClass<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>external-lb-1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">controller</span>:<span style="color:#bbb"> </span>example.com/ingress-controller<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">parameters</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># 이 인그레스클래스에 대한 파라미터는 "external-config-1" 라는</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># ClusterIngressParameter(API 그룹 k8s.example.net)에 기재되어 있다.</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># 이 정의는 쿠버네티스가 </span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># 클러스터 범위의 파라미터 리소스를 검색하도록 한다.</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">scope</span>:<span style="color:#bbb"> </span>Cluster<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">apiGroup</span>:<span style="color:#bbb"> </span>k8s.example.net<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>ClusterIngressParameter<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>external-config-1<span style="color:#bbb">
</span></code></pre></div></div>
<div id="tabs-ingressclass-parameter-scope-1" class="tab-pane" role="tabpanel" aria-labelledby="tabs-ingressclass-parameter-scope-1">
<p></p><div style="margin-top:10px;margin-bottom:10px">
<b>FEATURE STATE:</b> <code>Kubernetes v1.23 [stable]</code>
</div>
<p><code>.spec.parameters</code> 필드를 설정하고
<code>.spec.parameters.scope</code> 필드를 <code>Namespace</code>로 지정하면,
인그레스클래스는 네임스페이스 범위의 리소스를 참조한다.
사용하고자 하는 파라미터가 속한 네임스페이스를
<code>.spec.parameters</code> 의 <code>namespace</code> 필드에 설정해야 한다.</p>
<p>파라미터의 <code>kind</code>(+<code>apiGroup</code>)는
네임스페이스 범위의 API (예: 컨피그맵) 를 참조하며,
파라미터의 <code>name</code>은
<code>namespace</code>에 명시한 네임스페이스의 특정 리소스를 가리킨다.</p>
<p>네임스페이스 범위의 파라미터를 이용하여,
클러스터 운영자가 워크로드에 사용되는 환경 설정(예: 로드 밸런서 설정, API 게이트웨이 정의)에 대한 제어를 위임할 수 있다.
클러스터 범위의 파라미터를 사용했다면 다음 중 하나에 해당된다.</p>
<ul>
<li>다른 팀의 새로운 환경 설정 변경을 적용하려고 할 때마다
클러스터 운영 팀이 매번 승인을 해야 한다. 또는,</li>
<li>애플리케이션 팀이 클러스터 범위 파라미터 리소스를 변경할 수 있게 하는
<a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/">RBAC</a> 롤, 바인딩 등의 특별 접근 제어를
클러스터 운영자가 정의해야 한다.</li>
</ul>
<p>인그레스클래스 API 자신은 항상 클러스터 범위이다.</p>
<p>네임스페이스 범위의 파라미터를 참조하는 인그레스클래스 예시가
다음과 같다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:#00f;font-weight:700">---</span><span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>IngressClass<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>external-lb-2<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">controller</span>:<span style="color:#bbb"> </span>example.com/ingress-controller<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">parameters</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># 이 인그레스클래스에 대한 파라미터는 </span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># "external-configuration" 환경 설정 네임스페이스에 있는</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:#080;font-style:italic"># "external-config" 라는 IngressParameter(API 그룹 k8s.example.com)에 기재되어 있다.</span><span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">scope</span>:<span style="color:#bbb"> </span>Namespace<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">apiGroup</span>:<span style="color:#bbb"> </span>k8s.example.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>IngressParameter<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">namespace</span>:<span style="color:#bbb"> </span>external-configuration<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>external-config<span style="color:#bbb">
</span></code></pre></div></div></div>
<h3 id="사용중단-deprecated-어노테이션">사용중단(Deprecated) 어노테이션<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%82%AC%EC%9A%A9%EC%A4%91%EB%8B%A8-deprecated-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>쿠버네티스 1.18에 IngressClass 리소스 및 <code>ingressClassName</code> 필드가 추가되기
전에 인그레스 클래스는 인그레스에서
<code>kubernetes.io/ingress.class</code> 어노테이션으로 지정되었다. 이 어노테이션은
공식적으로 정의된 것은 아니지만, 인그레스 컨트롤러에서 널리 지원되었었다.</p>
<p>인그레스의 최신 <code>ingressClassName</code> 필드는 해당 어노테이션을
대체하지만, 직접적으로 해당하는 것은 아니다. 어노테이션은 일반적으로
인그레스를 구현해야 하는 인그레스 컨트롤러의 이름을 참조하는 데 사용되었지만,
이 필드는 인그레스 컨트롤러의 이름을 포함하는 추가 인그레스 구성이
포함된 인그레스 클래스 리소스에 대한 참조이다.</p>
<h3 id="default-ingress-class">기본 IngressClass<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#default-ingress-class" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>특정 IngressClass를 클러스터의 기본 값으로 표시할 수 있다. IngressClass
리소스에서 <code>ingressclass.kubernetes.io/is-default-class</code> 를 <code>true</code> 로
설정하면 <code>ingressClassName</code> 필드가 지정되지 않은
새 인그레스에게 기본 IngressClass가 할당된다.</p>
<div class="alert alert-warning caution callout" role="alert">
<strong>주의:</strong> 클러스터의 기본값으로 표시된 IngressClass가 두 개 이상 있는 경우
어드미션 컨트롤러에서 <code>ingressClassName</code> 이 지정되지 않은
새 인그레스 오브젝트를 생성할 수 없다. 클러스터에서 최대 1개의 IngressClass가
기본값으로 표시하도록 해서 이 문제를 해결할 수 있다.
</div>
<p>몇몇 인그레스 컨트롤러는 기본 <code>IngressClass</code>가 정의되어 있지 않아도 동작한다.
예를 들어, Ingress-NGINX 컨트롤러는 <code>--watch-ingress-without-class</code>
<a href="https://kubernetes.github.io/ingress-nginx/#what-is-the-flag-watch-ingress-without-class">플래그</a>를 이용하여 구성될 수 있다.
하지만 다음과 같이 기본 <code>IngressClass</code>를 명시하는 것을
<a href="https://kubernetes.github.io/ingress-nginx/#i-have-only-one-instance-of-the-ingresss-nginx-controller-in-my-cluster-what-should-i-do">권장</a>한다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/default-ingressclass.yaml" download="service/networking/default-ingressclass.yaml"><code>service/networking/default-ingressclass.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-default-ingressclass-yaml&#39;)" title="Copy service/networking/default-ingressclass.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-default-ingressclass-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>IngressClass<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">labels</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">app.kubernetes.io/component</span>:<span style="color:#bbb"> </span>controller<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>nginx-example<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">annotations</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">ingressclass.kubernetes.io/is-default-class</span>:<span style="color:#bbb"> </span><span style="color:#b44">"true"</span><span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">controller</span>:<span style="color:#bbb"> </span>k8s.io/ingress-nginx<span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<h2 id="인그레스-유형들">인그레스 유형들<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EC%9C%A0%ED%98%95%EB%93%A4" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<h3 id="single-service-ingress">단일 서비스로 지원되는 인그레스<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#single-service-ingress" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>단일 서비스를 노출할 수 있는 기존 쿠버네티스 개념이 있다
(<a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EB%8C%80%EC%95%88">대안</a>을 본다). 인그레스에 규칙 없이 <em>기본 백엔드</em> 를 지정해서
이를 수행할 수 있다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/test-ingress.yaml" download="service/networking/test-ingress.yaml"><code>service/networking/test-ingress.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-test-ingress-yaml&#39;)" title="Copy service/networking/test-ingress.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-test-ingress-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>test-ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">defaultBackend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>test<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<p>만약 <code>kubectl apply -f</code> 를 사용해서 생성한다면 추가한 인그레스의
상태를 볼 수 있어야 한다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-bash" data-lang="bash">kubectl get ingress test-ingress
</code></pre></div><pre><code>NAME           CLASS         HOSTS   ADDRESS         PORTS   AGE
test-ingress   external-lb   *       203.0.113.123   80      59s
</code></pre><p>여기서 <code>203.0.113.123</code> 는 인그레스 컨트롤러가 인그레스를 충족시키기 위해
할당한 IP 이다.</p>
<div class="alert alert-info note callout" role="alert">
<strong>참고:</strong> 인그레스 컨트롤러와 로드 밸런서는 IP 주소를 할당하는데 1~2분이 걸릴 수 있다.
할당될 때 까지는 주소는 종종 <code>&lt;pending&gt;</code> 으로 표시된다.
</div>
<h3 id="간단한-팬아웃-fanout">간단한 팬아웃(fanout)<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EA%B0%84%EB%8B%A8%ED%95%9C-%ED%8C%AC%EC%95%84%EC%9B%83-fanout" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>팬아웃 구성은 HTTP URI에서 요청된 것을 기반으로 단일 IP 주소에서 1개 이상의 서비스로
트래픽을 라우팅 한다. 인그레스를 사용하면 로드 밸런서의 수를
최소로 유지할 수 있다. 예를 들어 다음과 같은 설정을 한다.</p>
<figure>
<div class="mermaid" data-processed="true"><svg id="mermaid-1651936427886" width="100%" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="412" style="max-width: 877.875px;" viewBox="0 0 877.875 412"><style>#mermaid-1651936427886 {font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}#mermaid-1651936427886 .error-icon{fill:#552222;}#mermaid-1651936427886 .error-text{fill:#552222;stroke:#552222;}#mermaid-1651936427886 .edge-thickness-normal{stroke-width:2px;}#mermaid-1651936427886 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1651936427886 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1651936427886 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1651936427886 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1651936427886 .marker{fill:#333333;stroke:#333333;}#mermaid-1651936427886 .marker.cross{stroke:#333333;}#mermaid-1651936427886 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1651936427886 .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1651936427886 .cluster-label text{fill:#333;}#mermaid-1651936427886 .cluster-label span{color:#333;}#mermaid-1651936427886 .label text,#mermaid-1651936427886 span{fill:#333;color:#333;}#mermaid-1651936427886 .node rect,#mermaid-1651936427886 .node circle,#mermaid-1651936427886 .node ellipse,#mermaid-1651936427886 .node polygon,#mermaid-1651936427886 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1651936427886 .node .label{text-align:center;}#mermaid-1651936427886 .node.clickable{cursor:pointer;}#mermaid-1651936427886 .arrowheadPath{fill:#333333;}#mermaid-1651936427886 .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1651936427886 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1651936427886 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-1651936427886 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-1651936427886 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1651936427886 .cluster text{fill:#333;}#mermaid-1651936427886 .cluster span{color:#333;}#mermaid-1651936427886 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1651936427886 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-1651936427886 .plain&gt;*{fill:#ddd!important;stroke:#fff!important;stroke-width:4px!important;color:#000!important;}#mermaid-1651936427886 .plain span{fill:#ddd!important;stroke:#fff!important;stroke-width:4px!important;color:#000!important;}#mermaid-1651936427886 .k8s&gt;*{fill:#326ce5!important;stroke:#fff!important;stroke-width:4px!important;color:#fff!important;}#mermaid-1651936427886 .k8s span{fill:#326ce5!important;stroke:#fff!important;stroke-width:4px!important;color:#fff!important;}#mermaid-1651936427886 .cluster&gt;*{fill:#fff!important;stroke:#bbb!important;stroke-width:2px!important;color:#326ce5!important;}#mermaid-1651936427886 .cluster span{fill:#fff!important;stroke:#bbb!important;stroke-width:2px!important;color:#326ce5!important;}</style><g><g class="output"><g class="clusters"><g class="cluster" id="flowchart-클러스터-49" transform="translate(574.75,206)" style="opacity: 1;"><rect width="590.25" height="396" x="-295.125" y="-198"></rect><g class="label" transform="translate(0, -184)" id="mermaid-1651936427886Text"><g transform="translate(-27.6875,-12)"><foreignobject width="55.375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">클러스터</div></foreignobject></g></g></g></g><g class="edgePaths"><g class="edgePath LS-client LE-ingress" id="L-client-ingress" style="opacity: 1;"><path class="path" d="M108.203125,206L122.48828125,206C136.7734375,206,165.34375,206,193.9140625,206C222.484375,206,251.0546875,206,269.5065104166667,206C287.9583333333333,206,296.2916666666667,206,300.4583333333333,206L304.625,206" marker-end="url(#arrowhead89)" style="fill:none;stroke-width:2px;stroke-dasharray:3;"></path><defs><marker id="arrowhead89" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-ingress LE-service1" id="L-ingress-service1" style="opacity: 1;"><path class="path" d="M435.139960106383,184L452.87965425531917,172C470.61934840425533,160,506.0987367021277,136,530.6665558510639,124C555.234375,112,568.890625,112,575.71875,112L582.546875,112" marker-end="url(#arrowhead90)" style="fill:none"></path><defs><marker id="arrowhead90" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-ingress LE-service2" id="L-ingress-service2" style="opacity: 1;"><path class="path" d="M435.139960106383,228L452.87965425531917,240C470.61934840425533,252,506.0987367021277,276,530.6665558510639,288C555.234375,300,568.890625,300,575.71875,300L582.546875,300" marker-end="url(#arrowhead91)" style="fill:none"></path><defs><marker id="arrowhead91" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service1 LE-pod1" id="L-service1-pod1" style="opacity: 1;"><path class="path" d="M715.102227393617,90L724.6164394946809,85.83333333333333C734.1306515957446,81.66666666666667,753.1590757978723,73.33333333333333,766.8399545656029,69.16666666666667C780.5208333333334,65,788.8541666666666,65,793.0208333333334,65L797.1875,65" marker-end="url(#arrowhead92)" style="fill:none"></path><defs><marker id="arrowhead92" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service1 LE-pod2" id="L-service1-pod2" style="opacity: 1;"><path class="path" d="M715.102227393617,134L724.6164394946809,138.16666666666666C734.1306515957446,142.33333333333334,753.1590757978723,150.66666666666666,766.8399545656029,154.83333333333334C780.5208333333334,159,788.8541666666666,159,793.0208333333334,159L797.1875,159" marker-end="url(#arrowhead93)" style="fill:none"></path><defs><marker id="arrowhead93" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service2 LE-pod3" id="L-service2-pod3" style="opacity: 1;"><path class="path" d="M715.102227393617,278L724.6164394946809,273.8333333333333C734.1306515957446,269.6666666666667,753.1590757978723,261.3333333333333,766.8399545656029,257.1666666666667C780.5208333333334,253,788.8541666666666,253,793.0208333333334,253L797.1875,253" marker-end="url(#arrowhead94)" style="fill:none"></path><defs><marker id="arrowhead94" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service2 LE-pod4" id="L-service2-pod4" style="opacity: 1;"><path class="path" d="M715.102227393617,322L724.6164394946809,326.1666666666667C734.1306515957446,330.3333333333333,753.1590757978723,338.6666666666667,766.8399545656029,342.8333333333333C780.5208333333334,347,788.8541666666666,347,793.0208333333334,347L797.1875,347" marker-end="url(#arrowhead95)" style="fill:none"></path><defs><marker id="arrowhead95" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(193.9140625,206)" style="opacity: 1;"><g transform="translate(-60.7109375,-24)" class="label"><rect rx="0" ry="0" width="121.421875" height="48"></rect><foreignobject width="121.421875" height="48"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-client-ingress" class="edgeLabel L-LS-client&#39; L-LE-ingress">인그레스-매니지드 <br> 로드 밸런서</span></div></foreignobject></g></g><g class="edgeLabel" transform="translate(541.578125,112)" style="opacity: 1;"><g transform="translate(-15.7421875,-12)" class="label"><rect rx="0" ry="0" width="31.484375" height="24"></rect><foreignobject width="31.484375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-ingress-service1" class="edgeLabel L-LS-ingress&#39; L-LE-service1">/foo</span></div></foreignobject></g></g><g class="edgeLabel" transform="translate(541.578125,300)" style="opacity: 1;"><g transform="translate(-15.96875,-12)" class="label"><rect rx="0" ry="0" width="31.9375" height="24"></rect><foreignobject width="31.9375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-ingress-service2" class="edgeLabel L-LS-ingress&#39; L-LE-service2">/bar</span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service1-pod1" class="edgeLabel L-LS-service1&#39; L-LE-pod1"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service1-pod2" class="edgeLabel L-LS-service1&#39; L-LE-pod2"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service2-pod3" class="edgeLabel L-LS-service2&#39; L-LE-pod3"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service2-pod4" class="edgeLabel L-LS-service2&#39; L-LE-pod4"></span></div></foreignobject></g></g></g><g class="nodes"><g class="node k8s" id="flowchart-ingress-35" transform="translate(402.6171875,206)" style="opacity: 1;"><rect rx="0" ry="0" x="-97.9921875" y="-22" width="195.984375" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-87.9921875,-12)"><foreignobject width="175.984375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">인그레스, 178.91.123.132</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod1-42" transform="translate(821.03125,65)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-service1-37" transform="translate(664.8671875,112)" style="opacity: 1;"><rect rx="0" ry="0" x="-82.3203125" y="-22" width="164.640625" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-72.3203125,-12)"><foreignobject width="144.640625" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">서비스 service1:4200</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod2-44" transform="translate(821.03125,159)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod3-46" transform="translate(821.03125,253)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-service2-39" transform="translate(664.8671875,300)" style="opacity: 1;"><rect rx="0" ry="0" x="-82.3203125" y="-22" width="164.640625" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-72.3203125,-12)"><foreignobject width="144.640625" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">서비스 service2:8080</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod4-48" transform="translate(821.03125,347)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node plain" id="flowchart-client-34" transform="translate(58.1015625,206)" style="opacity: 1;"><rect rx="22" ry="22" x="-50.1015625" y="-22" width="100.203125" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-34.6015625,-12)"><foreignobject width="69.203125" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">클라이언트</div></foreignobject></g></g></g></g></g></g></svg></div>
</figure>
<noscript>
<div class="alert alert-secondary callout" role=alert>
<em class=javascript-required>JavaScript must be <a href=https://www.enable-javascript.com/>enabled</a> to view this content</em>
</div>
</noscript>
<p>다음과 같은 인그레스가 필요하다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/simple-fanout-example.yaml" download="service/networking/simple-fanout-example.yaml"><code>service/networking/simple-fanout-example.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-simple-fanout-example-yaml&#39;)" title="Copy service/networking/simple-fanout-example.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-simple-fanout-example-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>simple-fanout-example<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>foo.bar.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/foo<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service1<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">4200</span><span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/bar<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service2<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">8080</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<p><code>kubectl apply -f</code> 를 사용해서 인그레스를 생성 할 때 다음과 같다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-shell" data-lang="shell">kubectl describe ingress simple-fanout-example
</code></pre></div><pre><code>Name:             simple-fanout-example
Namespace:        default
Address:          178.91.123.132
Default backend:  default-http-backend:80 (10.8.2.3:8080)
Rules:
  Host         Path  Backends
  ----         ----  --------
  foo.bar.com
               /foo   service1:4200 (10.8.0.90:4200)
               /bar   service2:8080 (10.8.0.91:8080)
Events:
  Type     Reason  Age                From                     Message
  ----     ------  ----               ----                     -------
  Normal   ADD     22s                loadbalancer-controller  default/test
</code></pre><p>인그레스 컨트롤러는 서비스(<code>service1</code>, <code>service2</code>)가 존재하는 한,
인그레스를 만족시키는 특정한 로드 밸런서를 프로비저닝한다.
이렇게 하면, 주소 필드에서 로드 밸런서의 주소를
볼 수 있다.</p>
<div class="alert alert-info note callout" role="alert">
<strong>참고:</strong> 사용 중인 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers/">인그레스 컨트롤러</a>에
따라 default-http-backend
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/">서비스</a>를 만들어야 할 수도 있다.
</div>
<h3 id="이름-기반의-가상-호스팅">이름 기반의 가상 호스팅<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B4%EB%A6%84-%EA%B8%B0%EB%B0%98%EC%9D%98-%EA%B0%80%EC%83%81-%ED%98%B8%EC%8A%A4%ED%8C%85" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>이름 기반의 가상 호스트는 동일한 IP 주소에서 여러 호스트 이름으로 HTTP 트래픽을 라우팅하는 것을 지원한다.</p>
<figure>
<div class="mermaid" data-processed="true"><svg id="mermaid-1651936427934" width="100%" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="412" style="max-width: 967.40625px;" viewBox="0 0 967.40625 412"><style>#mermaid-1651936427934 {font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}#mermaid-1651936427934 .error-icon{fill:#552222;}#mermaid-1651936427934 .error-text{fill:#552222;stroke:#552222;}#mermaid-1651936427934 .edge-thickness-normal{stroke-width:2px;}#mermaid-1651936427934 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1651936427934 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1651936427934 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1651936427934 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1651936427934 .marker{fill:#333333;stroke:#333333;}#mermaid-1651936427934 .marker.cross{stroke:#333333;}#mermaid-1651936427934 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1651936427934 .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1651936427934 .cluster-label text{fill:#333;}#mermaid-1651936427934 .cluster-label span{color:#333;}#mermaid-1651936427934 .label text,#mermaid-1651936427934 span{fill:#333;color:#333;}#mermaid-1651936427934 .node rect,#mermaid-1651936427934 .node circle,#mermaid-1651936427934 .node ellipse,#mermaid-1651936427934 .node polygon,#mermaid-1651936427934 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1651936427934 .node .label{text-align:center;}#mermaid-1651936427934 .node.clickable{cursor:pointer;}#mermaid-1651936427934 .arrowheadPath{fill:#333333;}#mermaid-1651936427934 .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1651936427934 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1651936427934 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-1651936427934 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-1651936427934 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1651936427934 .cluster text{fill:#333;}#mermaid-1651936427934 .cluster span{color:#333;}#mermaid-1651936427934 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1651936427934 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-1651936427934 .plain&gt;*{fill:#ddd!important;stroke:#fff!important;stroke-width:4px!important;color:#000!important;}#mermaid-1651936427934 .plain span{fill:#ddd!important;stroke:#fff!important;stroke-width:4px!important;color:#000!important;}#mermaid-1651936427934 .k8s&gt;*{fill:#326ce5!important;stroke:#fff!important;stroke-width:4px!important;color:#fff!important;}#mermaid-1651936427934 .k8s span{fill:#326ce5!important;stroke:#fff!important;stroke-width:4px!important;color:#fff!important;}#mermaid-1651936427934 .cluster&gt;*{fill:#fff!important;stroke:#bbb!important;stroke-width:2px!important;color:#326ce5!important;}#mermaid-1651936427934 .cluster span{fill:#fff!important;stroke:#bbb!important;stroke-width:2px!important;color:#326ce5!important;}</style><g><g class="output"><g class="clusters"><g class="cluster" id="flowchart-클러스터-80" transform="translate(619.515625,206)" style="opacity: 1;"><rect width="679.78125" height="396" x="-339.890625" y="-198"></rect><g class="label" transform="translate(0, -184)" id="mermaid-1651936427934Text"><g transform="translate(-27.6875,-12)"><foreignobject width="55.375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">클러스터</div></foreignobject></g></g></g></g><g class="edgePaths"><g class="edgePath LS-client LE-ingress" id="L-client-ingress" style="opacity: 1;"><path class="path" d="M108.203125,206L122.48828125,206C136.7734375,206,165.34375,206,193.9140625,206C222.484375,206,251.0546875,206,269.5065104166667,206C287.9583333333333,206,296.2916666666667,206,300.4583333333333,206L304.625,206" marker-end="url(#arrowhead140)" style="fill:none;stroke-width:2px;stroke-dasharray:3;"></path><defs><marker id="arrowhead140" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-ingress LE-service1" id="L-ingress-service1" style="opacity: 1;"><path class="path" d="M447.5807845744681,184L472.1063829787234,172C496.6319813829787,160,545.6831781914893,136,585.8962765957447,124C626.109375,112,657.484375,112,673.171875,112L688.859375,112" marker-end="url(#arrowhead141)" style="fill:none"></path><defs><marker id="arrowhead141" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-ingress LE-service2" id="L-ingress-service2" style="opacity: 1;"><path class="path" d="M447.5807845744681,228L472.1063829787234,240C496.6319813829787,252,545.6831781914893,276,585.8962765957447,288C626.109375,300,657.484375,300,673.171875,300L688.859375,300" marker-end="url(#arrowhead142)" style="fill:none"></path><defs><marker id="arrowhead142" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service1 LE-pod1" id="L-service1-pod1" style="opacity: 1;"><path class="path" d="M809.0965757978723,90L817.8669381648937,85.83333333333333C826.637300531915,81.66666666666667,844.1780252659574,73.33333333333333,857.1150542996453,69.16666666666667C870.0520833333334,65,878.3854166666666,65,882.5520833333334,65L886.71875,65" marker-end="url(#arrowhead143)" style="fill:none"></path><defs><marker id="arrowhead143" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service1 LE-pod2" id="L-service1-pod2" style="opacity: 1;"><path class="path" d="M809.0965757978723,134L817.8669381648937,138.16666666666666C826.637300531915,142.33333333333334,844.1780252659574,150.66666666666666,857.1150542996453,154.83333333333334C870.0520833333334,159,878.3854166666666,159,882.5520833333334,159L886.71875,159" marker-end="url(#arrowhead144)" style="fill:none"></path><defs><marker id="arrowhead144" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service2 LE-pod3" id="L-service2-pod3" style="opacity: 1;"><path class="path" d="M809.0965757978723,278L817.8669381648937,273.8333333333333C826.637300531915,269.6666666666667,844.1780252659574,261.3333333333333,857.1150542996453,257.1666666666667C870.0520833333334,253,878.3854166666666,253,882.5520833333334,253L886.71875,253" marker-end="url(#arrowhead145)" style="fill:none"></path><defs><marker id="arrowhead145" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-service2 LE-pod4" id="L-service2-pod4" style="opacity: 1;"><path class="path" d="M809.0965757978723,322L817.8669381648937,326.1666666666667C826.637300531915,330.3333333333333,844.1780252659574,338.6666666666667,857.1150542996453,342.8333333333333C870.0520833333334,347,878.3854166666666,347,882.5520833333334,347L886.71875,347" marker-end="url(#arrowhead146)" style="fill:none"></path><defs><marker id="arrowhead146" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(193.9140625,206)" style="opacity: 1;"><g transform="translate(-60.7109375,-24)" class="label"><rect rx="0" ry="0" width="121.421875" height="48"></rect><foreignobject width="121.421875" height="48"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-client-ingress" class="edgeLabel L-LS-client&#39; L-LE-ingress">인그레스-매니지드 <br> 로드 밸런서</span></div></foreignobject></g></g><g class="edgeLabel" transform="translate(594.734375,112)" style="opacity: 1;"><g transform="translate(-69.125,-12)" class="label"><rect rx="0" ry="0" width="138.25" height="24"></rect><foreignobject width="138.25" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-ingress-service1" class="edgeLabel L-LS-ingress&#39; L-LE-service1">호스트: foo.bar.com</span></div></foreignobject></g></g><g class="edgeLabel" transform="translate(594.734375,300)" style="opacity: 1;"><g transform="translate(-69.125,-12)" class="label"><rect rx="0" ry="0" width="138.25" height="24"></rect><foreignobject width="138.25" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-ingress-service2" class="edgeLabel L-LS-ingress&#39; L-LE-service2">호스트: bar.foo.com</span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service1-pod1" class="edgeLabel L-LS-service1&#39; L-LE-pod1"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service1-pod2" class="edgeLabel L-LS-service1&#39; L-LE-pod2"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service2-pod3" class="edgeLabel L-LS-service2&#39; L-LE-pod3"></span></div></foreignobject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignobject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-service2-pod4" class="edgeLabel L-LS-service2&#39; L-LE-pod4"></span></div></foreignobject></g></g></g><g class="nodes"><g class="node k8s" id="flowchart-ingress-66" transform="translate(402.6171875,206)" style="opacity: 1;"><rect rx="0" ry="0" x="-97.9921875" y="-22" width="195.984375" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-87.9921875,-12)"><foreignobject width="175.984375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">인그레스, 178.91.123.132</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod1-73" transform="translate(910.5625,65)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-service1-68" transform="translate(762.7890625,112)" style="opacity: 1;"><rect rx="0" ry="0" x="-73.9296875" y="-22" width="147.859375" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-63.9296875,-12)"><foreignobject width="127.859375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">서비스 service1:80</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod2-75" transform="translate(910.5625,159)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod3-77" transform="translate(910.5625,253)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-service2-70" transform="translate(762.7890625,300)" style="opacity: 1;"><rect rx="0" ry="0" x="-73.9296875" y="-22" width="147.859375" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-63.9296875,-12)"><foreignobject width="127.859375" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">서비스 service2:80</div></foreignobject></g></g></g><g class="node k8s" id="flowchart-pod4-79" transform="translate(910.5625,347)" style="opacity: 1;"><rect rx="0" ry="0" x="-23.84375" y="-22" width="47.6875" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-13.84375,-12)"><foreignobject width="27.6875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">파드</div></foreignobject></g></g></g><g class="node plain" id="flowchart-client-65" transform="translate(58.1015625,206)" style="opacity: 1;"><rect rx="22" ry="22" x="-50.1015625" y="-22" width="100.203125" height="44" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-34.6015625,-12)"><foreignobject width="69.203125" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">클라이언트</div></foreignobject></g></g></g></g></g></g></svg></div>
</figure>
<noscript>
<div class="alert alert-secondary callout" role=alert>
<em class=javascript-required>JavaScript must be <a href=https://www.enable-javascript.com/>enabled</a> to view this content</em>
</div>
</noscript>
<p>다음 인그레스는 <a href="https://tools.ietf.org/html/rfc7230#section-5.4">호스트 헤더</a>에 기반한 요청을
라우팅 하기 위해 뒷단의 로드 밸런서를 알려준다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/name-virtual-host-ingress.yaml" download="service/networking/name-virtual-host-ingress.yaml"><code>service/networking/name-virtual-host-ingress.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-name-virtual-host-ingress-yaml&#39;)" title="Copy service/networking/name-virtual-host-ingress.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-name-virtual-host-ingress-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>name-virtual-host-ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>foo.bar.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service1<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>bar.foo.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service2<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<p>만약 규칙에 정의된 호스트 없이 인그레스 리소스를 생성하는 경우,
이름 기반 가상 호스트가 없어도 인그레스 컨트롤러의 IP 주소에 대한 웹
트래픽을 일치 시킬 수 있다.</p>
<p>예를 들어, 다음 인그레스는 <code>first.bar.com</code>에 요청된 트래픽을
<code>service1</code>로, <code>second.bar.com</code>는 <code>service2</code>로, 그리고 요청 헤더가 <code>first.bar.com</code> 또는 <code>second.bar.com</code>에 해당되지 않는 모든 트래픽을 <code>service3</code>로 라우팅한다.</p>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/name-virtual-host-ingress-no-third-host.yaml" download="service/networking/name-virtual-host-ingress-no-third-host.yaml"><code>service/networking/name-virtual-host-ingress-no-third-host.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-name-virtual-host-ingress-no-third-host-yaml&#39;)" title="Copy service/networking/name-virtual-host-ingress-no-third-host.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-name-virtual-host-ingress-no-third-host-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>name-virtual-host-ingress-no-third-host<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>first.bar.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service1<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>second.bar.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service2<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span><span style="color:#b44">"/"</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service3<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<h3 id="tls">TLS<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#tls" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>TLS 개인 키 및 인증서가 포함된 <a class="glossary-tooltip" title="" data-toggle="tooltip" data-placement="top" href="https://kubernetes.io/ko/docs/concepts/configuration/secret/" target="_blank" aria-label="시크릿(Secret)" data-original-title="비밀번호, OAuth 토큰 및 ssh 키와 같은 민감한 정보를 저장한다.">시크릿(Secret)</a>을
지정해서 인그레스를 보호할 수 있다. 인그레스 리소스는
단일 TLS 포트인 443만 지원하고 인그레스 지점에서 TLS 종료를
가정한다(서비스 및 해당 파드에 대한 트래픽은 일반 텍스트임).
인그레스의 TLS 구성 섹션에서 다른 호스트를 지정하면, SNI TLS 확장을 통해
지정된 호스트이름에 따라 동일한 포트에서 멀티플렉싱
된다(인그레스 컨트롤러가 SNI를 지원하는 경우). TLS secret에는
<code>tls.crt</code> 와 <code>tls.key</code> 라는 이름의 키가 있어야 하고, 여기에는 TLS에 사용할 인증서와
개인 키가 있다. 예를 들어 다음과 같다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Secret<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>testsecret-tls<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">namespace</span>:<span style="color:#bbb"> </span>default<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">data</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">tls.crt</span>:<span style="color:#bbb"> </span>base64 encoded cert<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">tls.key</span>:<span style="color:#bbb"> </span>base64 encoded key<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">type</span>:<span style="color:#bbb"> </span>kubernetes.io/tls<span style="color:#bbb">
</span></code></pre></div><p>인그레스에서 시크릿을 참조하면 인그레스 컨트롤러가 TLS를 사용하여
클라이언트에서 로드 밸런서로 채널을 보호하도록 지시한다. 생성한
TLS 시크릿이 <code>https-example.foo.com</code> 의 정규화 된 도메인 이름(FQDN)이라고
하는 일반 이름(CN)을 포함하는 인증서에서 온 것인지 확인해야 한다.</p>
<div class="alert alert-info note callout" role="alert">
<strong>참고:</strong> 가능한 모든 하위 도메인에 대해 인증서가 발급되어야 하기 때문에
TLS는 기본 규칙에서 작동하지 않는다. 따라서
<code>tls</code> 섹션의 <code>hosts</code>는 <code>rules</code>섹션의 <code>host</code>와 명시적으로 일치해야
한다.
</div>
<div class="highlight">
<div class="copy-code-icon" style="text-align:right">
<a href="https://raw.githubusercontent.com/kubernetes/website/main/content/ko/examples/service/networking/tls-example-ingress.yaml" download="service/networking/tls-example-ingress.yaml"><code>service/networking/tls-example-ingress.yaml</code>
</a>
<img src="./ingress_kubernetes_files/copycode.svg" style="max-height:24px;cursor:pointer" onclick="copyCode(&#39;service-networking-tls-example-ingress-yaml&#39;)" title="Copy service/networking/tls-example-ingress.yaml to clipboard">

</div>
<div class="includecode" id="service-networking-tls-example-ingress-yaml">
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">apiVersion</span>:<span style="color:#bbb"> </span>networking.k8s.io/v1<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">kind</span>:<span style="color:#bbb"> </span>Ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">metadata</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>tls-example-ingress<span style="color:#bbb">
</span><span style="color:#bbb"></span><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">tls</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">hosts</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- https-example.foo.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">secretName</span>:<span style="color:#bbb"> </span>testsecret-tls<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>https-example.foo.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service1<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span></code></pre></div>
</div>
</div>
<div class="alert alert-info note callout" role="alert">
<strong>참고:</strong> TLS 기능을 제공하는 다양한 인그레스 컨트롤러간의 기능
차이가 있다. 사용자 환경에서의 TLS의 작동 방식을 이해하려면
<a href="https://kubernetes.github.io/ingress-nginx/user-guide/tls/">nginx</a>,
<a href="https://git.k8s.io/ingress-gce/README.md#frontend-https">GCE</a> 또는 기타
플랫폼의 특정 인그레스 컨트롤러에 대한 설명서를 참조한다.
</div>
<h3 id="load-balancing">로드 밸런싱<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#load-balancing" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h3>
<p>인그레스 컨트롤러는 로드 밸런싱 알고리즘, 백엔드 가중치 구성표 등
모든 인그레스에 적용되는 일부 로드 밸런싱
정책 설정으로 부트스트랩된다. 보다 진보된 로드 밸런싱 개념
(예: 지속적인 세션, 동적 가중치)은 아직 인그레스를 통해
노출되지 않는다. 대신 서비스에 사용되는 로드 밸런서를 통해 이러한 기능을
얻을 수 있다.</p>
<p>또한, 헬스 체크를 인그레스를 통해 직접 노출되지 않더라도, 쿠버네티스에는
<a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/">준비 상태 프로브</a>와
같은 동일한 최종 결과를 얻을 수 있는 병렬 개념이
있다는 점도 주목할 가치가 있다. 컨트롤러 별
설명서를 검토하여 헬스 체크를 처리하는 방법을 확인한다(예:
<a href="https://git.k8s.io/ingress-nginx/README.md">nginx</a>, 또는
<a href="https://git.k8s.io/ingress-gce/README.md#health-checks">GCE</a>).</p>
<h2 id="인그레스-업데이트">인그레스 업데이트<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>기존 인그레스를 업데이트해서 새 호스트를 추가하려면, 리소스를 편집해서 호스트를 업데이트 할 수 있다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-shell" data-lang="shell">kubectl describe ingress <span style="color:#a2f">test</span>
</code></pre></div><pre><code>Name:             test
Namespace:        default
Address:          178.91.123.132
Default backend:  default-http-backend:80 (10.8.2.3:8080)
Rules:
  Host         Path  Backends
  ----         ----  --------
  foo.bar.com
               /foo   service1:80 (10.8.0.90:80)
Annotations:
  nginx.ingress.kubernetes.io/rewrite-target:  /
Events:
  Type     Reason  Age                From                     Message
  ----     ------  ----               ----                     -------
  Normal   ADD     35s                loadbalancer-controller  default/test
</code></pre><div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-shell" data-lang="shell">kubectl edit ingress <span style="color:#a2f">test</span>
</code></pre></div><p>YAML 형식의 기존 구성이 있는 편집기가 나타난다.
새 호스트를 포함하도록 수정한다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-yaml" data-lang="yaml"><span style="color:green;font-weight:700">spec</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span><span style="color:green;font-weight:700">rules</span>:<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>foo.bar.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service1<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/foo<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb">  </span>- <span style="color:green;font-weight:700">host</span>:<span style="color:#bbb"> </span>bar.baz.com<span style="color:#bbb">
</span><span style="color:#bbb">    </span><span style="color:green;font-weight:700">http</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span><span style="color:green;font-weight:700">paths</span>:<span style="color:#bbb">
</span><span style="color:#bbb">      </span>- <span style="color:green;font-weight:700">backend</span>:<span style="color:#bbb">
</span><span style="color:#bbb">          </span><span style="color:green;font-weight:700">service</span>:<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">name</span>:<span style="color:#bbb"> </span>service2<span style="color:#bbb">
</span><span style="color:#bbb">            </span><span style="color:green;font-weight:700">port</span>:<span style="color:#bbb">
</span><span style="color:#bbb">              </span><span style="color:green;font-weight:700">number</span>:<span style="color:#bbb"> </span><span style="color:#666">80</span><span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">path</span>:<span style="color:#bbb"> </span>/foo<span style="color:#bbb">
</span><span style="color:#bbb">        </span><span style="color:green;font-weight:700">pathType</span>:<span style="color:#bbb"> </span>Prefix<span style="color:#bbb">
</span><span style="color:#bbb"></span>..<span style="color:#bbb">
</span></code></pre></div><p>변경사항을 저장한 후, kubectl은 API 서버의 리소스를 업데이트하며, 인그레스
컨트롤러에게도 로드 밸런서를 재구성하도록 지시한다.</p>
<p>이것을 확인한다.</p>
<div class="highlight"><pre tabindex="0" style="background-color:#f8f8f8;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-shell" data-lang="shell">kubectl describe ingress <span style="color:#a2f">test</span>
</code></pre></div><pre><code>Name:             test
Namespace:        default
Address:          178.91.123.132
Default backend:  default-http-backend:80 (10.8.2.3:8080)
Rules:
  Host         Path  Backends
  ----         ----  --------
  foo.bar.com
               /foo   service1:80 (10.8.0.90:80)
  bar.baz.com
               /foo   service2:80 (10.8.0.91:80)
Annotations:
  nginx.ingress.kubernetes.io/rewrite-target:  /
Events:
  Type     Reason  Age                From                     Message
  ----     ------  ----               ----                     -------
  Normal   ADD     45s                loadbalancer-controller  default/test
</code></pre><p>수정된 인그레스 YAML 파일을 <code>kubectl replace -f</code> 를 호출해서 동일한 결과를 얻을 수 있다.</p>
<h2 id="가용성-영역에-전체에서의-실패">가용성 영역에 전체에서의 실패<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EA%B0%80%EC%9A%A9%EC%84%B1-%EC%98%81%EC%97%AD%EC%97%90-%EC%A0%84%EC%B2%B4%EC%97%90%EC%84%9C%EC%9D%98-%EC%8B%A4%ED%8C%A8" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>장애 도메인에 트래픽을 분산시키는 기술은 클라우드 공급자마다 다르다.
자세한 내용은 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers">인그레스 컨트롤러</a> 설명서를 확인한다.</p>
<h2 id="대안">대안<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EB%8C%80%EC%95%88" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<p>사용자는 인그레스 리소스를 직접적으로 포함하지 않는 여러가지 방법으로 서비스를 노출할 수 있다.</p>
<ul>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/#loadbalancer">Service.Type=LoadBalancer</a> 사용.</li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/#type-nodeport">Service.Type=NodePort</a> 사용.</li>
</ul>
<h2 id="다음-내용">다음 내용<a aria-hidden="true" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EB%8B%A4%EC%9D%8C-%EB%82%B4%EC%9A%A9" style="visibility: hidden;"> <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" width="24" height="24" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"></path><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"></path></svg></a></h2>
<ul>
<li><a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/">인그레스</a> API에 대해 배우기</li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers/">인그레스 컨트롤러</a>에 대해 배우기</li>
<li><a href="https://kubernetes.io/ko/docs/tasks/access-application-cluster/ingress-minikube/">NGINX 컨트롤러로 Minikube에서 인그레스 구성하기</a></li>
</ul>
</div>
<div id="pre-footer">
<h2>피드백</h2>
<p class="feedback--prompt">이 페이지가 도움이 되었나요? </p>
<button class="btn btn-primary mb-4 feedback--yes">네</button>
<button class="btn btn-primary mb-4 feedback--no">아니요</button>
<p class="feedback--response feedback--response__hidden">
피드백 감사합니다. 쿠버네티스 사용 방법에 대해서 구체적이고 답변 가능한 질문이 있다면, 다음 링크에서 질문하십시오.
<a target="_blank" rel="noopener" href="https://stackoverflow.com/questions/tagged/kubernetes">
Stack Overflow</a>.
원한다면 GitHub 리포지터리에 이슈를 열어서
<a class="feedback--link" target="_blank" rel="noopener" href="https://github.com/kubernetes/website/issues/new?title=Issue%20with%20k8s.io/ko/docs/concepts/services-networking/ingress/">
문제 리포트</a>
또는
<a class="feedback--link" target="_blank" rel="noopener" href="https://github.com/kubernetes/website/issues/new?title=Improvement%20for%20k8s.io/ko/docs/concepts/services-networking/ingress/">
개선 제안이 가능합니다.</a>.
</p>
</div>
<script>const yes=document.querySelector('.feedback--yes'),no=document.querySelector('.feedback--no');document.querySelectorAll('.feedback--link').forEach(a=>{a.href=a.href+window.location.pathname});const sendFeedback=a=>{gtag||console.log('!gtag'),gtag('event','click',{event_category:'Helpful',event_label:window.location.pathname,value:a})},disableButtons=()=>{yes.disabled=!0,yes.classList.add('feedback--button__disabled'),no.disabled=!0,no.classList.add('feedback--button__disabled')};yes.addEventListener('click',()=>{sendFeedback(1),disableButtons(),document.querySelector('.feedback--response').classList.remove('feedback--response__hidden')}),no.addEventListener('click',()=>{sendFeedback(0),disableButtons(),document.querySelector('.feedback--response').classList.remove('feedback--response__hidden')})</script>
<div class="text-muted mt-5 pt-3 border-top">
최종 수정 March 25, 2022 at 10:37 AM PST: <a href="https://github.com/kubernetes/website/commit/9e6b2ef46af9e5c5ff24cdd5c71ffc17e091dfe2">[ko] Update links in dev-1.23-ko.2 (9e6b2ef46a)</a>
</div>
</main>
<div class="d-none d-xl-block col-xl-2 td-toc d-print-none">
<div class="td-page-meta ml-2 pb-1 pt-2 mb-0">
<a href="https://github.com/kubernetes/website/edit/main/content/ko/docs/concepts/services-networking/ingress.md" target="_blank"><i class="fa fa-edit fa-fw"></i> 페이지 편집</a>
<a href="https://github.com/kubernetes/website/new/main/content/ko/docs/concepts/services-networking/ingress.md?filename=change-me.md&amp;value=---%0Atitle%3A+%22Long+Page+Title%22%0AlinkTitle%3A+%22Short+Nav+Title%22%0Aweight%3A+100%0Adescription%3A+%3E-%0A+++++Page+description+for+heading+and+indexes.%0A---%0A%0A%23%23+Heading%0A%0AEdit+this+template+to+create+your+new+page.%0A%0A%2A+Give+it+a+good+name%2C+ending+in+%60.md%60+-+e.g.+%60getting-started.md%60%0A%2A+Edit+the+%22front+matter%22+section+at+the+top+of+the+page+%28weight+controls+how+its+ordered+amongst+other+pages+in+the+same+directory%3B+lowest+number+first%29.%0A%2A+Add+a+good+commit+message+at+the+bottom+of+the+page+%28%3C80+characters%3B+use+the+extended+description+field+for+more+detail%29.%0A%2A+Create+a+new+branch+so+you+can+preview+your+new+file+and+request+a+review+via+Pull+Request.%0A" target="_blank"><i class="fa fa-edit fa-fw"></i> 하부 페이지 생성</a>
<a href="https://github.com/kubernetes/website/issues/new?title=%ec%9d%b8%ea%b7%b8%eb%a0%88%ec%8a%a4%28Ingress%29" target="_blank"><i class="fab fa-github fa-fw"></i> 이슈 생성</a>
<a id="print" href="https://kubernetes.io/ko/docs/concepts/services-networking/_print/"><i class="fa fa-print fa-fw"></i> 전체 섹션 프린트</a>
</div>
<nav id="TableOfContents">
<ul>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9A%A9%EC%96%B4">용어</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4%EB%9E%80">인그레스란?</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%A0%84%EC%A0%9C-%EC%A1%B0%EA%B1%B4%EB%93%A4">전제 조건들</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EB%A6%AC%EC%86%8C%EC%8A%A4">인그레스 리소스</a>
<ul>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EA%B7%9C%EC%B9%99">인그레스 규칙</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#default-backend">DefaultBackend</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#resource-backend">리소스 백엔드</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EA%B2%BD%EB%A1%9C-%EC%9C%A0%ED%98%95">경로 유형</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%98%88%EC%A0%9C">예제</a></li>
</ul>
</li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%ED%98%B8%EC%8A%A4%ED%8A%B8%EB%84%A4%EC%9E%84-%EC%99%80%EC%9D%BC%EB%93%9C%EC%B9%B4%EB%93%9C">호스트네임 와일드카드</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%ED%81%B4%EB%9E%98%EC%8A%A4">인그레스 클래스</a>
<ul>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%B2%94%EC%9C%84">인그레스클래스 범위</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%82%AC%EC%9A%A9%EC%A4%91%EB%8B%A8-deprecated-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98">사용중단(Deprecated) 어노테이션</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#default-ingress-class">기본 IngressClass</a></li>
</ul>
</li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EC%9C%A0%ED%98%95%EB%93%A4">인그레스 유형들</a>
<ul>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#single-service-ingress">단일 서비스로 지원되는 인그레스</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EA%B0%84%EB%8B%A8%ED%95%9C-%ED%8C%AC%EC%95%84%EC%9B%83-fanout">간단한 팬아웃(fanout)</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B4%EB%A6%84-%EA%B8%B0%EB%B0%98%EC%9D%98-%EA%B0%80%EC%83%81-%ED%98%B8%EC%8A%A4%ED%8C%85">이름 기반의 가상 호스팅</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#tls">TLS</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#load-balancing">로드 밸런싱</a></li>
</ul>
</li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8">인그레스 업데이트</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EA%B0%80%EC%9A%A9%EC%84%B1-%EC%98%81%EC%97%AD%EC%97%90-%EC%A0%84%EC%B2%B4%EC%97%90%EC%84%9C%EC%9D%98-%EC%8B%A4%ED%8C%A8">가용성 영역에 전체에서의 실패</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EB%8C%80%EC%95%88">대안</a></li>
<li><a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#%EB%8B%A4%EC%9D%8C-%EB%82%B4%EC%9A%A9">다음 내용</a></li>
</ul>
</nav>
</div>
</div>
</div>
</div>
<footer class="d-print-none">
<div class="footer__links">
<nav>
<a class="text-white" href="https://kubernetes.io/ko/docs/home/">홈</a>
<a class="text-white" href="https://kubernetes.io/ko/blog/">블로그</a>
<a class="text-white" href="https://kubernetes.io/ko/training/">교육</a>
<a class="text-white" href="https://kubernetes.io/ko/partners/">파트너</a>
<a class="text-white" href="https://kubernetes.io/ko/community/">커뮤니티</a>
<a class="text-white" href="https://kubernetes.io/ko/case-studies/">사례 연구</a>
</nav>
</div>
<div class="container-fluid">
<div class="row">
<div class="col-6 col-sm-2 text-xs-center order-sm-2">
<ul class="list-inline mb-0">
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="User mailing list" data-original-title="User mailing list">
<a class="text-white" target="_blank" href="https://discuss.kubernetes.io/">
<i class="fa fa-envelope"></i>
</a>
</li>
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="Twitter" data-original-title="Twitter">
<a class="text-white" target="_blank" href="https://twitter.com/kubernetesio">
<i class="fab fa-twitter"></i>
</a>
</li>
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="Calendar" data-original-title="Calendar">
<a class="text-white" target="_blank" href="https://calendar.google.com/calendar/embed?src=calendar%40kubernetes.io">
<i class="fas fa-calendar-alt"></i>
</a>
</li>
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="Youtube" data-original-title="Youtube">
<a class="text-white" target="_blank" href="https://youtube.com/kubernetescommunity">
<i class="fab fa-youtube"></i>
</a>
</li>
</ul>
</div>
<div class="col-6 col-sm-2 text-right text-xs-center order-sm-3">
<ul class="list-inline mb-0">
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="GitHub" data-original-title="GitHub">
<a class="text-white" target="_blank" href="https://github.com/kubernetes/kubernetes">
<i class="fab fa-github"></i>
</a>
</li>
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="Slack" data-original-title="Slack">
<a class="text-white" target="_blank" href="https://slack.k8s.io/">
<i class="fab fa-slack"></i>
</a>
</li>
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="Contribute" data-original-title="Contribute">
<a class="text-white" target="_blank" href="https://git.k8s.io/community/contributors/guide">
<i class="fas fa-edit"></i>
</a>
</li>
<li class="list-inline-item mx-2 h3" data-toggle="tooltip" data-placement="top" title="" aria-label="Stack Overflow" data-original-title="Stack Overflow">
<a class="text-white" target="_blank" href="https://stackoverflow.com/questions/tagged/kubernetes">
<i class="fab fa-stack-overflow"></i>
</a>
</li>
</ul>
</div>
<div class="col-12 col-sm-8 text-center order-sm-2">
<small class="text-white">© 2022 The Kubernetes Authors | Documentation Distributed under <a href="https://git.k8s.io/website/LICENSE" class="light-text">CC BY 4.0</a></small>
<br>
<small class="text-white">Copyright © 2022 The Linux Foundation ®. All rights reserved. The Linux Foundation has registered trademarks and uses trademarks. For a list of trademarks of The Linux Foundation, please see our <a href="https://www.linuxfoundation.org/trademark-usage" class="light-text">Trademark Usage page</a></small>
<br>
<small class="text-white">ICP license: 京ICP备17074266号-3</small>
</div>
</div>
</div>
</footer>
<script src="./ingress_kubernetes_files/popper-1.14.3.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="./ingress_kubernetes_files/bootstrap-4.3.1.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
<script src="./ingress_kubernetes_files/main.min.40616251a9b6e4b689e7769be0340661efa4d7ebb73f957404e963e135b4ed52.js" integrity="sha256-QGFiUam25LaJ53ab4DQGYe+k1+u3P5V0BOlj4TW07VI=" crossorigin="anonymous"></script>
<script async="" src="./ingress_kubernetes_files/sweetalert-2.1.2.min.js"></script>
<script type="text/javascript">function copyCode(c){var d,a,b;if(document.getElementById(c)){d="_hiddenCopyText_",a=document.getElementById(d),a||(a=document.createElement("textarea"),a.style.position="absolute",a.style.left="-9999px",a.style.top="0",a.id=d,document.body.appendChild(a)),a.value=document.getElementById(c).innerText,a.select();try{b=document.execCommand("copy")}catch(a){swal("Oh, no…","Sorry, your browser doesn't support copying this example to your clipboard."),b=!1}return b?(swal("Copied to clipboard: ",c),b):(swal("Oops!",c+" not found when trying to copy code"),!1)}}</script>

<div class="pi-pushmenu" id="primary" style="display: none;"><div class="overlay"><div class="sled" style="right: 0px;"><div class="content"><ul><li><a class="navbar-brand" href="https://kubernetes.io/ko/" id=""></a></li><li><a class="nav-link active" href="https://kubernetes.io/ko/docs/" id="">문서</a></li><li><a class="nav-link" href="https://kubernetes.io/ko/blog/" id="">쿠버네티스 블로그</a></li><li><a class="nav-link" href="https://kubernetes.io/ko/training/" id="">교육</a></li><li><a class="nav-link" href="https://kubernetes.io/ko/partners/" id="">파트너</a></li><li><a class="nav-link" href="https://kubernetes.io/ko/community/" id="">커뮤니티</a></li><li><a class="nav-link" href="https://kubernetes.io/ko/case-studies/" id="">사례 연구</a></li><li><a class="nav-link dropdown-toggle" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#" id="" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
버전
</a></li><li><a class="dropdown-item" href="https://kubernetes.io/releases" id="">Release Information</a></li><li><a class="dropdown-item" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/" id="">v1.24</a></li><li><a class="dropdown-item" href="https://v1-23.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/" id="">v1.23</a></li><li><a class="dropdown-item" href="https://v1-22.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/" id="">v1.22</a></li><li><a class="dropdown-item" href="https://v1-21.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/" id="">v1.21</a></li><li><a class="dropdown-item" href="https://v1-20.docs.kubernetes.io/ko/docs/concepts/services-networking/ingress/" id="">v1.20</a></li><li><a class="nav-link dropdown-toggle" href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/#" id="" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
한국어 Korean
</a></li><li><a class="dropdown-item" href="https://kubernetes.io/docs/concepts/services-networking/ingress/" id="">English</a></li><li><a class="dropdown-item" href="https://kubernetes.io/zh/docs/concepts/services-networking/ingress/" id="">中文 Chinese</a></li><li><a class="dropdown-item" href="https://kubernetes.io/ja/docs/concepts/services-networking/ingress/" id="">日本語 Japanese</a></li><li><a class="dropdown-item" href="https://kubernetes.io/fr/docs/concepts/services-networking/ingress/" id="">Français</a></li><li><a class="dropdown-item" href="https://kubernetes.io/id/docs/concepts/services-networking/ingress/" id="">Bahasa Indonesia</a></li></ul></div><button class="push-menu-close-button"></button></div></div></div><div class="mermaidTooltip" style="opacity: 0;"></div></body></html>
