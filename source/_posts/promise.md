---
title: ES6系列-promise
categories: js
tags: [编程,学习]
---
```javascript
// promise
{
    //传统es5回调方式解决异步
    let ajax = function (callback) {
        console.log('等待下一步加载。。。')
        setTimeout(() => {
            callback && callback.call()
        }, 1000);
    }
    ajax(function () {
        console.log('来了来了')
    })
}

{
    //执行a-b-c-d-e-f....
    let ajax = () => {
        console.log('等待setTimeOut2加载')
        return new Promise((resolve, reject) => {  //执行，中断
            setTimeout(() => {
                resolve()
            }, 1000);
        })
    }
    ajax().then(() => {
        console.log('promise', 'setTimeOut2')
    })
}

{
    let ajax = () => {
        console.log('等待第一次加载')
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve()
            }, 1000);
        })
    }
    ajax().then(() => {
        console.log('等待第二次')
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve()
            }, 2000);
        })
    })
        .then(() => {
            console.log('我是第三次加载')
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve()
                }, 3000);
            })
        })
        .then(() => {
            console.log('最后一次加载')
        })
}
//如果中间出现错误，如何捕获
{
    let ajax = function (num) {
        console.log('判断num')
        return new Promise((resolve, reject) => {
            if (num < 5) {
                resolve()
            } else {
                throw new Error('传错了')
            }
        })
    }
    ajax(6).then(() => {
        console.log('log', 6)
    }).catch(err => {
        console.log('catch', err)
    })
    ajax(2).then(() => {
        console.log('log', 2)
    }).catch(err => {
        console.log('catch', err)
    })
}

//使用场景promise.all,promise
{
    // 所有图都加载后显示到页面
    function loadImg(src) {
        return new Promise((resolve, reject) => {
            let img = document.createElement('img')
            img.src = src
            img.onload = () => {
                resolve(img)
            }
            img.onerror = () => {
                reject(err)
            }
            //图片加载完成onload
        })
    }
    //添加到页面
    function showImg(imgs) {
        imgs.forEach(item => {
            document.body.appendChild(item)
        })
    }
    // promise.all把多个promise实例当成一个promise实例
    Promise.all([
        loadImg('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545736426664&di=219db669df4a813bcca53e582da7582e&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201706%2F12%2F20170612130531_wXcaQ.thumb.700_0.jpeg'),
        loadImg('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545736426664&di=2c20d892a361fa4313ae380600c434b8&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw440h329%2F20171229%2Fbe5c-fypyuve2937030.jpg'),
        loadImg('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545736426663&di=1ce727cefe866cd3baa0bcdc64dd8cb7&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201706%2F12%2F20170612130914_CrVz5.jpeg')
    ]).then(showImg)
}
{
    //先到先得,有一本图片加载完就加载到页面
    function loadImg(src) {
        return new Promise((resolve, reject) => {
            let img = document.createElement('img')
            img.src = src
            img.onload = () => {
                resolve(img)
            }
            img.onerror = () => {
                reject(err)
            }
        })
    }

    function showImg(imgs) {
        document.body.appendChild(imgs)
    }
    Promise.race([
        loadImg('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545736426664&di=219db669df4a813bcca53e582da7582e&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201706%2F12%2F20170612130531_wXcaQ.thumb.700_0.jpeg'),
        loadImg('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545736426664&di=2c20d892a361fa4313ae380600c434b8&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw440h329%2F20171229%2Fbe5c-fypyuve2937030.jpg'),
        loadImg('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545736426663&di=1ce727cefe866cd3baa0bcdc64dd8cb7&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201706%2F12%2F20170612130914_CrVz5.jpeg')
    ]).then(showImg)

}
```