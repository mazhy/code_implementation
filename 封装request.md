# 封装request
```js
// 封装一个axios的实例
import Axios from 'axios';
import { serverHost, port } from '../config';

// 创建一个新的实例
let r = Axios.create({
    baseURL: `${serverHost}:${port}/`,
    withCredentials: true, // 允许客户端再跨域的时候携带cookie
});

// 发请求之前(自动添加头信息)
r.interceptors.request.use(function(config) {
    return config;
});


// 定义响应的拦截器
r.interceptors.response.use(function(res) {
    if (res.data.errcode == 10001) {
        alert('没有登录,无法访问数据');
        // window.location.href = '/signin';
    }
    return res;
});

let request = function(url = '', options = {}) {
    return function() {
        // /signin
        if (!(url!= '/signin'||url != '/signout')) {
            let isLogin = JSON.parse(window.sessionStorage.getItem('user') || 'false');
            if (!isLogin) {
                alert('请求拦截器拦截了请求');
                // window.location.href = '/signin';
                return Promise.reject('没有登录 不能发请求');
            }
        }
  


        if (url === '') return Promise.reject('必须传递url');
        // 将promise对象返回去
        return r({
            url,
            method: 'get', // method先给一个get请求
            ...options, // options中有method就会覆盖
        });

    }
}

export default request;
```