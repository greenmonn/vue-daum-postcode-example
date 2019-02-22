# Vue Daum PostCode API 예제

Daum에서 제공하는 우편번호/주소 검색 API를 Vue로 작성한 Single File Component에서 활용하는 예제입니다. 

[공식 사용 가이드](http://postcode.map.daum.net/guide)에서 제공된 plain javascript 예제를 Vue에 맞게 변형했습니다.

## Web Component
결과를 Web Component로 build한 결과를 함께 제공합니다.

[http-server](https://www.npmjs.com/package/http-server)로 간단히 실행해 볼 수 있습니다.

```bash
http-server wc/
```

이후 `localhost:8080/postcode.html`에서 확인 가능합니다.

## Import CDN script
동작하기 위해서는 daum에서 CDN으로 제공하는 script를 페이지 내부에 삽입해야 합니다.

```html
<script src="http://dmaps.daum.net/map_js_init/postcode.v2.js"></script>
```

Vue CLI 3을 사용하고 있다면, `index.html` 파일이 노출되어 있지 않기 때문에 전역 script를 삽입하기 어려울 수 있습니다.

[html-webpack-externals-plugin](https://www.npmjs.com/package/html-webpack-externals-plugin)을 사용하는 것을 권장합니다.

플러그인을 설치한 이후 이 경우 `vue.config.js` 파일에 다음과 같이 추가하면 됩니다.

```javascript
const HtmlWebpackExternalsPlugin = require('html-webpack-externals-plugin');

module.exports = {
  configureWebpack: {
    plugins: [
      new HtmlWebpackExternalsPlugin({
        externals: [
          {
            module: 'daum-postcode-api',
            entry: 'http://dmaps.daum.net/map_js_init/postcode.v2.js',
            global: 'daum-postcode-api',
          },
        ],
      }),
    ],
  },
};

```

## TODO
추후 Vue Plugin 형태로 사용할 수 있도록 개선할 예정입니다.

- 참고: [다음 우편번호 서비스 사용 가이드](http://postcode.map.daum.net/guide)