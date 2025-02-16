---
title: Angular使用
copyright: false
date: 2019-05-23 13:33:00
tags: 
 - angular
categories: 
 - 编程
---
## 初始化环境
```bash
npm install -g @angular/cli     #安装全局命令行
ng new ngapp                    #生成一个新项目
ng serve --open                 #启动服务器，热刷新
ng serve --proxy-config proxy.conf.json --host 0.0.0.0 --disable-host-check --port 4200 --live-reload-port 4201 #详细设置相关参数
ng serve --prod --env=dev --proxy-config proxy.conf.json --host 0.0.0.0 --disable-host-check
# 这是生产构建
ng build --target=production --environment=prod
ng build --prod --env=prod
ng build --prod
# 这是开发构建
ng build --target=development --environment=dev
ng build --dev --e=dev
ng build --dev
```

## 常用开发命令-组件、服务、路由
```bash
ng generate component heroes        #生成一个heroes组件，CLI 创建了一个新的文件夹 src/app/heroes/
ng generate component hero -it      #取消模板文件,创建内联模块
ng g c --inline-template=true --inline-style=true tab   #创建内联模块
ng generate service hero            #创建一个名叫 hero 的服务
ng generate service hero --module=app   #创建服务,并把服务注入App组件中
    #把提供商添加到根模块上，以便在任何地方都使用服务的同一个实例
ng generate module app-routing --flat --module=app #创建路由模块 
    #--flat 把这个文件放进了 src/app 中，而不是单独的目录中
    #--module=app 告诉 CLI 把它注册到 AppModule 的 imports 数组中
ng generate class hero              #创建一个类
#参考地址：https://github.com/angular/angular-cli/wiki/generate-component
#参考地址：https://www.npmjs.com/package/angular-cli-tools?activeTab=readme
```

## 页面常用指令
```bash
{{ hero.name | uppercase }}     #过滤器，转为大写，lowercase小写
{{nullHero?.name}}              #替换*ngIf检测空，a?.b?.c?.d
{{$any(hero).marker}}           #{{$any(this).member}},访问组件中未声明过的成员
(click)="onSelect(hero)"        #事件绑定方式
<a routerLink="/heroes">H</a>   #路由跳转方式
    
[(ngModel)]="hero.name"         #input的数据双向绑定
    #这两个的简写[ngModel]="hero.name" (ngModelChange)="setUppercaseName($event)"
    #import { FormsModule } from '@angular/forms';imports: [FormsModule]需要导入对应的包
[ngClass]="currentClasses"      #绑定类样式 currentClasses = {'special':  this.isSpecial};
[class.special]="isSpecial"     #单个样式开关
[ngStyle]="currentStyles"       #绑定style对象{'font-style': this.canSave  ? 'italic' : 'normal'}
[style.display]="isSpecial ? 'block' : 'none'"  #简单样式
[style.visibility]="isFirstVote?'visible':'hidden'"
[innerHTML]="aa"                #插入HTML	

*ngFor="let hero of heroes;let i = index"       #for循环语句
[ngSwitch]="currentHero.emotion" --> *ngSwitchCase="'happy'" --> *ngSwitchDefault

#input事件
(focus)="getMatchInfo()" #聚焦时执行 (blur)="getMatchInfo()" 失焦时执行

#组件内引入内容
<ng-content select="[ion-fixed],ion-fab"></ng-content>
#空占位符
<ng-container></ng-container>
#模板语法,封装指令,[IF]等
<ng-template></ng-template>
```

## Angular常用TS
```bash
#import {Input} from '@angular/core';
@Input() hero: Hero;            #父子组件通信方式，数据双向绑定,可放set方法上  @Input() set name(name: string){}
@Output('myClick') clicks       #绑定通知事件，并起别名‘myClick’
@ViewChild(ChildenComponent) child: ChildenComponent;   #子组件实例引用
@ViewChild("child") child2;     #字符串,指向含有‘#child’的元素
@ViewChild('childB', {read: ElementRef})    #匹配元素,html元素
@ViewChild('childB', {read: ViewContainerRef})  #匹配元素,匹配视图容器
@ViewChild("refresher") divTop: ElementRef;     #获取html元素
<input #phone placeholder="phone number">       #模板引用变量,(click)="callPhone(phone.value)" === ref-phone
#heroForm="ngForm" --> <button type="submit" [disabled]="!heroForm.form.valid">Submit</button>

#滚动到指定锚点
@ViewChild("top",{read: ElementRef}) ticketEle;
this.top && this.top.nativeElement.scrollIntoView (true);
#滚动到选中的tab
scrollToSelect() {
    let selected = this.ticketEle.nativeElement.querySelector(".select");
    if(selected) {
      let ofTop = selected.offsetTop;
      this.scroll.nativeElement.scrollTo(0,ofTop);
    }
}

#克隆页面元素
elementRef.nativeElement.cloneNode(true)
#移除元素
element.parentNode && element.parentNode.removeChild(element);

#允许自定义元素，icon-content等icon-header
@NgModule({ schemas: [CUSTOM_ELEMENTS_SCHEMA] })

#事件监听一
import {Observable} from "rxjs";
Observable.fromEvent(this.ele.nativeElement, 'scroll').subscribe((event) => {
  console.log('scroll',32);
});
#事件监听二
import {Renderer2} from "@angular/core";
this.renderer.listen(this.ele.nativeElement, 'click', () => {
  console.log('click',27);
});

//js阻止事件冒泡
oEvent.cancelBubble = true;
oEvent.stopPropagation();

//js阻止链接默认行为，没有停止冒泡
oEvent.preventDefault(); 

#检测数据变化
import {ChangeDetectorRef} from '@angular/core';
public cdf: ChangeDetectorRef
this.cdf.detectChanges();

#angular6补充
ng g pipe service/date --module ../router/bootstrap #以服务所在目录为根目录确定模块
ng g service service/http       #新建服务，可用模块名替代root
```

## ES6常用语法
```bash
heroes => this.heroes = heroes  #只有一个返回语句的函数
{ name } as Hero                #把Name的值赋给Hero对象的Name属性
{bbc}                           #把bbc变量变成键值对bbc-value对象	
arrs.find(item => item === 1)   #数组查找指定条件的值
heroes.filter(h => h !== hero)  #过滤数组，true时过滤掉,类似删除
oldArray.map(entry => {'abc:' + entry;})    #生成新的数组，可把数组转对象等
/**
 * 排序，false:倒序,默认从大到小
 * @param {string} prop,{boolean} sc asc/desc
 * @returns {any}
 * this.sortArray('d30WinRatio',false);
 */
sortArray(prop: string,sc:boolean=true) {
    const sorted = this.list.sort((a, b) => a[prop] > b[prop] ? 1 : a[prop] === b[prop] ? 0 : -1);
    if(!sc) sorted.reverse();
    return sorted;
}

#对象转数组--{a:1,b:2,c:3}--->[1,2,3]
let arr = Object.keys(this.typeData).map(key=> this.typeData[key]);

#对象拷贝增量赋值
class C{
    a;
    b;
    getA(){return this.a}
    getB(){return this.b}
}
let tmp = new C();
let obj = {a:6,b:7};
var res = Object.assign(tmp,obj,{c:8});//{a:6,b:7,c:8}; res === tmp;
```

## 打包和优化
```bash
ng build --prod --bh ./         #指定base href的值编译
ng build –prod –aot             #不压缩编译，生成map文件
ng build --prod --stats-json    #输出包体组成分析文件，生成 stats.json
ng build --prod --build-optimizer   #配合 UglifyJs 能够智能的移除未使用代码
ng eject                        #导出Webpack配置,.angular-cli.json添加"ejected": true
```

## Angular的Rest风格部署
```bash
#静态网站，配置Nginx
location / {
    root   html/dist;
    try_files $uri $uri/ /index.html =404; #主要是这句
    index  index.html index.htm;
}

#动态网站
app.use(function (req, res) {
    console.log(req.path);
    if(req.path.indexOf('/api')>=0){
        res.send("server text");
    }else{ //angular启动页
        res.sendfile('app/index.html');
    }
});
```

## 常用开发技巧
```bash
#命令行扩展工具
npm install angular-cli-tools -g    #命令行扩展工具
ngt g class [class-name]            #创建类
ngt g c [component-name]            #创建组建
ngt g d [directive-name]            #创建指令
ngt g e [enum-name]                 #创建枚举
ngt g h [name]                      #创建html
ngt g index                         #创建索引		
ngt g i [interface-name]            #创建接口
ngt g m [module-name]               #创建模块
ngt g p [pipe-name]                 #创建管道
ngt g r [route-name]                #创建动态路由
ngt g routing [routing-name]        #创建静态路由
ngt g s [service-name]e]            #创建服务
ngt g style [style-name]            #创建样式

#添加延迟加载路由
import { NgModule }  from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { ActivityComponent } from './activity.component';
const routes: Routes = [
  { path: '', component: ActivityComponent },
];
@NgModule({
  imports: [ RouterModule.forChild(routes)],
  exports: [ RouterModule ]
})
export class ActivityRouting{};

/*
{path: 'activity', loadChildren: './activity/activity.module#ActivityModule'},
*/

#常用示列
ngt update index --recursive    #更新当前目录和子目录的index索引
#模版文件安装配置
ngt install config              #在项目跟路径创建配置文件
ngt s ./login-form.module.ts -t:form-module #使用指定文件创建form模版
ngt g m test -t:form-module     #使用刚刚创建的模版去创建test模块
#ng更新后的命令，可以在当前目录创建对应的模块
ng update @angular/cli
ng g c --inline-template=true --inline-style=true tab   #创建内联tab模块
ng g c tab -its 		#上面命令简写

#路由参数获取
#route:ActivatedRoute 获取路由参数
route.snapshot.params['roomStatus']
route.queryParams['roomStatus']
this.router.navigate(['/activity/puzzles/ranking', {uid: 123, tid: res.model.id}])
routerLink="/activity/puzzles"
this.router.navigate(['/login'],{ skipLocationChange: true });	#防止重复跳登陆页面，url不变
this.router.navigate(['./ranking'],{replaceUrl:true,relativeTo:this.currentRoute});//Url改变,不添加到历史记录

#监听当前页面路由变化，可以监听到历史回退
import {debounceTime, filter, map, mergeMap} from "rxjs/operators";
#第一次加载不会触发--可放入init里面
this.subRouter = this.router.events
  .pipe(filter(event => event instanceof NavigationEnd),map(() => this.route))
  .pipe(map(route => {
    while (route.firstChild) route = route.firstChild;
    return route;
  }))
  .pipe(filter(route => route.outlet === 'primary'))
  .pipe(mergeMap(route => route.params),debounceTime(300))
  .subscribe((event) =>{
    if(Number(event.uid) && Number(event.type)){
      this.uid = +event.uid;
      this.type = +event.type;
    }
  });

#同路由页面替换URL参数，第一个不添加到历史记录，第二个会产生历史记录
if (!!(window.history && history.pushState)){
  history.replaceState({uid: this.uid, type: this.type}, '活动排名页', `#/activity/puzzles/ranking;uid=${this.uid};type=${this.type}`);
}else{
  //this.router.navigate(['/activity/puzzles/ranking', {uid: this.uid, type: this.type}]);
  this.router.navigate(['./ranking'],{replaceUrl:true,relativeTo:this.currentRoute});//不添加到历史记录
}

//rxjs防抖动函数
import {Subject} from "rxjs/Subject";
import {debounceTime, distinctUntilChanged} from 'rxjs/operators';
changeStream: Subject<string> = new Subject<string>();
this.order = this.order ||
      this.changeStream
          .pipe(debounceTime(2000))
          .pipe(distinctUntilChanged())
          .subscribe(streetText => {
              //实际业务处理
              console.log(38);
          });
#调用方式
this.changeStream.next(this.list);
```

## 常用指令写法
```bash
#时间格式化
import { DatePipe } from '@angular/common';
private datePipe: DatePipe
this.datePipe.transform(this.ticketInfo.flyTime, 'yyyy-MM-dd HH:mm')

#ng-container：特别对for循环和if同时使用时，特别有效
既不是一个Component，也不是一个Directive，只是单纯的一个特殊tag。ng-container可以直接包裹任何元素，包括文本，但本身不会生成元素标签，也不会影响页面样式和布局。包裹的内容，如果不通过其他指令控制，会直接渲染到页面中

https://map.baidu.com/?qt=cur&wd=%E6%B7%B1%E5%9C%B3%E5%B8%82  //天气预报接口
https://map.baidu.com/mobile/?qt=loc&x=113.9278992&y=22.543741&pois=1 //地理信息接口
```
## IphoneX的样式兼容
```bash
@media only screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) {
    	.iphoneTop .indexTop,
	    .iphoneTop .liveTop,
	    .iphoneTop .navBar{ padding-top: 44px;}
	    .betBottomBar,.messageFixed{ bottom: 34px;}
	    .iphoneTop .fixed_top_bar .title{ margin-top: 44px;}
	}

#背景图平铺
.content-bg{background: url(../images/bg_repeat.jpg) repeat-y;background-size: 100% auto;}

#浏览器自带滚动条隐藏
html::-webkit-scrollBar{display:none;}

#解决输入框自动补全，黄色背景
input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus,
input:-webkit-autofill:active {
    -webkit-box-shadow: 0 0 0px 1000px #333 inset;
    transition: background-color 50000s ease-in-out 0s, color 5000s ease-in-out 0s;
}

#这种可以解决回退后，显示黄色的背景，完美解决
input:-webkit-autofill {
   -webkit-animation: autofill-fix 1s infinite;
}
@-webkit-keyframes autofill-fix {
    from {
        background-color: transparent
    }
    to {
        background-color: transparent
    }
}

#加上spinner类能让p和div等块旋转
.spinner{
    -webkit-animation: spin 1s linear 1s 5 alternate;
    animation: spin 1s linear infinite;
    display: inline-block;
}
@-webkit-keyframes spin {
    from {
        -webkit-transform: rotate(0deg);
    }
    to {
        -webkit-transform: rotate(360deg);
    }
}

@keyframes spin {
    from {
        transform: rotate(0deg);
    }
    to {
        transform: rotate(360deg);
    }
}

#rem单位初始化--Ts
declare var window:any;
initWinPage(){
    this.restWinPage(window, window.lib || (window.lib = {}));
}

private restWinPage(N, M) {
    function L() {
        let a = I.getBoundingClientRect().width;
        a / F > 540 && (a = 540 * F);
        let d = a / 7.5;
        I.style.fontSize = d + "px", D.rem = N.rem = d
    }

    let K, J = N.document, I = J.documentElement, H = J.querySelector('meta[name="viewport"]'),
        G = J.querySelector('meta[name="flexible"]'), F = 0, E = 0, D = M.flexible || (M.flexible = {});
    if (H) {
        // console.warn("将根据已有的meta标签来设置缩放比例");
        let C = H.getAttribute("content").match(/initial\-scale=([\d\.]+)/);
        C && (E = parseFloat(C[1]), F = parseInt(''+1 / E))
    } else {
        if (G) {
            let B = G.getAttribute("content");
            if (B) {
                let A = B.match(/initial\-dpr=([\d\.]+)/), z = B.match(/maximum\-dpr=([\d\.]+)/);
                A && (F = parseFloat(A[1]), E = parseFloat((1 / F).toFixed(2))), z && (F = parseFloat(z[1]), E = parseFloat((1 / F).toFixed(2)))
            }
        }
    }
    if (!F && !E) {
        let y = N.navigator.userAgent, x = (!!y.match(/android/gi) && !!y.match(/iphone/gi)),
            w = x && !!y.match(/OS 9_3/), v = N.devicePixelRatio;
        F = x ? v >= 3 && (!F || F >= 3) ? 3 : v >= 2 && (!F || F >= 2) ? 2 : 1 : 1, E = 1 / F
    }
    if (I.setAttribute("data-dpr", F), !H) {
        if (H = J.createElement("meta"), H.setAttribute("name", "viewport"), H.setAttribute("content", "initial-scale=" + E + ", maximum-scale=" + E + ", minimum-scale=" + E + ", user-scalable=no"), I.firstElementChild) {
            I.firstElementChild.appendChild(H)
        } else {
            let u = J.createElement("div");
            u.appendChild(H), J.write(u.innerHTML)
        }
    }
    N.addEventListener("resize", function () {
        clearTimeout(K), K = setTimeout(L, 300)
    }, !1), N.addEventListener("pageshow", function (b) {
        b.persisted && (clearTimeout(K), K = setTimeout(L, 300))
    }, !1), "complete" === J.readyState ? J.body.style.fontSize = 12 * F + "px" : J.addEventListener("DOMContentLoaded", function () {
        J.body.style.fontSize = 12 * F + "px"
    }, !1), L(), D.dpr = N.dpr = F, D.refreshRem = L, D.rem2px = function (d) {
        let c:any = parseFloat(d) * this.rem;
        return "string" == typeof d && d.match(/rem$/) && (c += "px"), c
    }, D.px2rem = function (d) {
        let c:any = parseFloat(d) / this.rem;
        return "string" == typeof d && d.match(/px$/) && (c += "rem"), c
    }
}

#iframe缩放问题
-webkit-transform: scaleY(0.7);//Y轴方向，缩放0.6倍
-webkit-transform-origin: 100% 100%;//缩放在右下角对齐
margin-top: -50px;//负值margin从而把其位置摆正确，有时需要放到包在div的外面
margin-left: -95px;//如果用scale全部缩放，需要设置这个值
#缩放示例
[style.marginTop.px]="cartoonHeight*0.2*-1"
[style.marginLeft.px]="cartoonWidth*0.2*-1"
style="transform: scale(0.8);-webkit-transform: scale(0.8);-webkit-transform-origin: 100% 100%;"
```

## SASS常用技巧
```bash
node-sass scss/app.scss css/app.css --output-style compressed	#编译并压缩代码，-w监听
node-sass -w -r scss -o css --output-style compressed 			#监听scss目录，编译到css目录
```

## cookie相关设置
```bash
#获取根域名，以便设置到根域名上，如 .baidu.com .google.com
function GetCookieDomain() {
    var host = location.hostname;
    var ip = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/;
    if (ip.test(host) === true || host === 'localhost') return host;
    var regex = /([^]*).*/;
    var match = host.match(regex);
    if (typeof match !== "undefined" && null !== match) host = match[1];
    if (typeof host !== "undefined" && null !== host) {
        var strAry = host.split(".");
        if (strAry.length > 1) {
            host = strAry[strAry.length - 2] + "." + strAry[strAry.length - 1];
        }
    }
    return '.' + host;
}

#设置cookie
document.cookie = cname + "=" + cvalue + "; expires=" + expires + "; domain=" + GetCookieDomain() + "; path=/";

#过期，当前时间减去一秒，立即过期
expires = (new Date().getTime() - 1000);
document.cookie = "agentId" + "=" + "123" + "; expires=" + (new Date().getTime() - 1000) + "; domain=" + GetCookieDomain() + "; path=/";
```