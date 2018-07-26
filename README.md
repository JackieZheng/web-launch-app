# web-launch-app
awake app from webpage

## Installation
npm install --save web-launch-app

## Usage
```javascript
import {LaunchApp} from 'web-launch-app';
const lanchApp = new LaunchApp({
    // protocol://path?param&param
    scheme: {
        android: {
            // page name(default:index)
            index: {
                protocol: 'tbfrs',
                path: 'tieba.baidu.com',
                // fixed parameter for this page
                param: {},
                // param map(to solve the problem of using different parameter names for different platforms)
                paramMap: {
                }
            },
            frs: {
                protocol: 'tbfrs',
                path: 'tieba.baidu.com',
                param: {},
                paramMap: {
                    forumName: 'kw'
                }
            }
        },
        ios: {
            index: {
                protocol: 'com.baidu.tieba',
                path: 'jumptoforum',
                param: {},
                paramMap: {
                }
            },
            frs: {
                protocol: 'com.baidu.tieba',
                path: 'jumptoforum',
                paramMap: {
                    forumName: 'tname'
                }
            }
        }
    },
    univerlink: {
        index: {
            url: 'https://tieba.baidu.com',
            param: {
            },
            paramMap: {
                forumName: 'kw'
            }
        },
        frs: {
            // support placeholder
            url: 'https://tieba.baidu.com/p/{kw}',
            param: {
            },
            paramMap: {
                forumName: 'kw'
            }
        }
    },
    yingyongbao: {
        url: 'http://a.app.qq.com/o/simple.jsp',
        param: {
            pkgname: 'com.baidu.tieba'
        }
    },
    pkgs: {
        androidApk: {
            // detector.browser.name
            default: 'https://downpack.baidu.com/baidutieba_AndroidPhone_v8.8.8.6(8.8.8.6)_1020584c.apk',
            qq: 'https://downpack.baidu.com/baidutieba_AndroidPhone_v8.8.8.6(8.8.8.6)_1020584c.apk',
            micromessenger:'https://downpack.baidu.com/baidutieba_AndroidPhone_v8.8.8.6(8.8.8.6)_1020584c.apk'
        },
        appstore: {
            default: 'https://itunes.apple.com/app/apple-store/id477927812?pt=328057&ct=MobileQQ_LXY&mt=8',
        },
        yingyongbao: {
            default: 'http://a.app.qq.com/o/simple.jsp?pkgname=com.baidu.tieba&ckey=CK1374101624513',
        }
    },
    // use UniversalLink for ios9+(default:true)
    useUniversalLink: true,
    // to yingyongbao in wechat
    useYingyongbao: false,
    // open tip in wechat
    wxGuideMethod: function (detector) {
        const explorerName = (detector.os.name == 'ios' ? 'Safari' : '');
        const div = document.createElement('div');
        div.style.position = 'absolute'
        div.style.top = '0';
        div.style.zIndex = '1111';
        div.style.width = '100%';
        div.style.height = '100%';
        div.innerHTML = `<div style="height:100%;background-color:#000;opacity:0.5;"></div>
        <div style="position:absolute;top:0;background: url(`
            + require('../static/img/finger.png') + ') no-repeat right top;background-size:' + px('204px') + ' ' + px('212px') + ';width:100%;padding-top:' + px('208px') + ';color:white;font-size:' + px('32px') + ';text-align:center;">'
            + (explorerName ? '<img src="' + require("../static/img/safari.png") + '" style="width:' + px('120px') + ';height:' + px('120px') + ';"/>' : '')
            + '<p style="font-size:' + px('36px') + ';font-weight:bold;margin-bottom:6px;">点右上角选择“在' + explorerName + '浏览器中打开”</p>'
            + '<p> 就能马上打开Nani了哦~</p></div>';
        document.body.appendChild(div);
        div.onclick = function () {
            div.remove()
        }
    },
    // download page url（boot the user to download or download installation packages directly）,jump to download page when it cant't find a corresponding configuration or get a error
    downPage: 'http://tieba.baidu.com/mo/q/activityDiversion/download',
    // the parameter prefix(default is question mark)
    searchPrefix: (detector) => {
       return '?';
    },
    // for scheme(default:2000)
    timeout: 3000
});
lanchApp.open();
lanchApp.open({
    openMethod:'yingyongbao'    //'weixin'|'yingyongbao'|'scheme'|'univerlink'
});
lanchApp.open({
    page: 'frs',
    param: {
        forumName: 'jawidx'
    },
    url:'https://tieba.baidu.com/jawidx'
});
lanchApp.open({
    page: 'frs',
    param: {
        forumName: 'jawidx'
    }
}, (status, detector) => {
    // status(0:failed，1:success，2:unknow)
    // detector()
    console.log('callback,', status, detector);
    // true: will down package when open failed or unknow
    return true;
});
lanchApp.down();
lanchApp.down({
    pkgs:{
        ios:'',
        android:''
    }
});
```
