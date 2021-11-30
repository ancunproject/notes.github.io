<template>
  <div id='jswbox'>
    <div class="prev" id="prev">
      <i class="iconfont icon-jiantou-zuo"></i>
    </div>
    <div class="next" id="next">
      <i class="iconfont icon-jiantou-you"></i>
    </div>
    <ul>
      <li data-desc="公安部销售许可证" data-sub-desc="（计算机信息系统安全专用产品销售许可证）"><img src='../static/img/honor/honor-1.png' /></li>
      <li data-desc="公安部计算机信息系统安全产品完整性鉴别检测报告"><img src='../static/img/honor/honor-2.png' /></li>
      <li data-desc="质量管理体系认证证书"><img src='../static/img/honor/honor-3.png' /></li>
      <li data-desc="信息安全管理体系认证证书"><img src='../static/img/honor/honor-4.png' /></li>
      <li data-desc="知识产权管理体系认证证书"><img src='../static/img/honor/honor-5.png' /></li>
      <li data-desc="电子签名授权书（CFCA）"><img src='../static/img/honor/honor-6.png' /></li>
      <li data-desc="国家工信部——安存电子证据平台测评报告"><img src='../static/img/honor/honor-7.png' /></li>
      <li data-desc="安存电子数据证据保全系统等级保护测评报告"><img src='../static/img/honor/honor-8.png' /></li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {}
  },
  mounted() {
    const Vue = this;
    function ZoomPic() { this.initialize.apply(this, arguments) }
    ZoomPic.prototype = {
      initialize: function (id) {
        let _this = this;
        this.wrap = typeof id === 'string' ? document.getElementById(id) : id;
        this.oUl = this.wrap.getElementsByTagName('ul')[0];
        this.aLi = this.wrap.getElementsByTagName('li');
        this.prev = document.getElementById('prev');
        this.next = document.getElementById('next');
        this.timer = null; this.aSort = []; this.iCenter = 3;
        this._doPrev = function () { return _this.doPrev.apply(_this) };
        this._doNext = function () { return _this.doNext.apply(_this) };
        this.options = [
          { width: 178, height: 248, top: 125, left: 0, zIndex: 1 },
          { width: 178, height: 248, top: 125, left: 0, zIndex: 2 },
          { width: 178, height: 248, top: 125, left: 200, zIndex: 3 },
          { width: 275, height: 383, top: 0, left: 400, zIndex: 4 },
          { width: 178, height: 248, top: 125, left: 695, zIndex: 3 },
          { width: 178, height: 248, top: 125, left: 895, zIndex: 2 },
          { width: 178, height: 248, top: 125, left: 895, zIndex: 1 }];
        for (let i = 0; i < this.aLi.length; i++) this.aSort[i] = this.aLi[i];
        this.aSort.unshift(this.aSort.pop());
        this.setUp();
        this.addEvent(this.prev, 'click', this._doPrev);
        this.addEvent(this.next, 'click', this._doNext);
        this.doImgClick();
        this.timer = setInterval(function () { _this.doNext() }, 3000);
        this.wrap.onmouseover = function () { clearInterval(_this.timer) };
        this.wrap.onmouseout = function () {
          _this.timer = setInterval(function () { _this.doNext() }, 3000);
        }
      },
      doPrev: function () {
        this.aSort.unshift(this.aSort.pop());
        this.setUp()
      },
      doNext: function () {
        this.aSort.push(this.aSort.shift());
        this.setUp()
      },
      doImgClick: function () {
        let _this = this;
        for (let i = 0; i < this.aSort.length; i++) {
          this.aSort[i].onclick = function () {
            if (this.index > _this.iCenter) {
              for (let i = 0; i < this.index - _this.iCenter; i++) {
                _this.aSort.push(_this.aSort.shift());
                _this.setUp();
              }
            } else if (this.index < _this.iCenter) {
              for (let i = 0; i < _this.iCenter - this.index; i++) {
                _this.aSort.unshift(_this.aSort.pop());
                _this.setUp();
              }
            }
          }
        }
      },
      setUp: function () {
        let _this = this;
        const desc = this.aSort[3].getAttribute('data-desc');
        Vue.$emit('updateDesc', { desc });
        let i = 0;
        for (i = 0; i < this.aSort.length; i++) this.oUl.appendChild(this.aSort[i]); for (i = 0; i < this.aSort.length; i++) {
          this.aSort[i].index = i; if (i < 7) {
            this.css(this.aSort[i], 'display', 'block');

            this.doMove(this.aSort[i], this.options[i], function () {
              _this.doMove(_this.aSort[_this.iCenter].getElementsByTagName('img')[0], { opacity: 100 }, function () {
                _this.doMove(_this.aSort[_this.iCenter].getElementsByTagName('img')[0], { opacity: 100 }, function () {
                  _this.aSort[_this.iCenter].onmouseover = function () {
                    _this.doMove(this.getElementsByTagName('div')[0], { bottom: 0 })
                  };
                  _this.aSort[_this.iCenter].onmouseout = function () {
                    _this.doMove(this.getElementsByTagName('div')[0], { bottom: -100 });
                  }
                })
              })
            });
          } else { this.css(this.aSort[i], 'display', 'none'); this.css(this.aSort[i], 'width', 0); this.css(this.aSort[i], 'height', 0); this.css(this.aSort[i], 'top', 152); this.css(this.aSort[i], 'left', this.oUl.offsetWidth / 2) }
          if (i < this.iCenter || i > this.iCenter) {
            this.css(this.aSort[i].getElementsByTagName('img')[0], 'opacity', 30)
            this.aSort[i].onmouseover = function () { _this.doMove(this.getElementsByTagName('img')[0], { opacity: 100 }) }; this.aSort[i].onmouseout = function () { _this.doMove(this.getElementsByTagName('img')[0], { opacity: 35 }) }; this.aSort[i].onmouseout();
          } else { this.aSort[i].onmouseover = this.aSort[i].onmouseout = null }
        }
      },
      addEvent: function (oElement, sEventType, fnHandler) {
        return oElement.addEventListener ? oElement.addEventListener(sEventType, fnHandler, false) : oElement.attachEvent('on' + sEventType, fnHandler)
      },
      css: function (oElement, attr, value) {
        if (arguments.length == 2) {
          return oElement.currentStyle ? oElement.currentStyle[attr] : getComputedStyle(oElement, null)[attr];
        } else if (arguments.length == 3) {
          switch (attr) { case 'width': case 'height': case 'top': case 'left': case 'bottom': oElement.style[attr] = value + 'px'; break; default: oElement.style[attr] = value; break }
        }
      },
      doMove: function (oElement, oAttr, fnCallBack) {
        let _this = this; clearInterval(oElement.timer);
        oElement.timer = setInterval(function () {
          let bStop = true;
          for (let property in oAttr) {
            let iCur = parseFloat(_this.css(oElement, property));
            property == 'opacity' && (iCur = parseInt(iCur.toFixed(2) * 100));
            let iSpeed = (oAttr[property] - iCur) / 5;
            iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
            if (iCur != oAttr[property]) {
              bStop = false; _this.css(oElement, property, iCur + iSpeed)
            }
          }
          if (bStop) {
            clearInterval(oElement.timer);
            fnCallBack && fnCallBack.apply(_this, arguments)
          }
        }, 30)
      }
    };
    /* eslint-disable no-new */
    new ZoomPic('jswbox');
  },
  methods: {}
}
</script>

<style lang='scss' scoped>
* {
  margin: 0;
  padding: 0;
  list-style-type: none;
}
a,
img {
  border: 0;
}
body {
  font: 12px/180% Arial, Helvetica, sans-serif, "������";
}

/* jswbox */
#jswbox {
  width: 1070px;
  height: 355px;
  margin: 0 auto;
  position: relative;
  text-align: center;
}
#jswbox ul {
  position: relative;
  height: 355px;
}
#jswbox li {
  position: absolute;
  width: 0;
  height: 0;
  z-index: 0;
  cursor: pointer;
  overflow: hidden;
  top: 152px;
  left: 20px;
}
#jswbox li img {
  width: 100%;
  height: 100%;
  vertical-align: top;
  float: left;
}
.prev,
.next {
  position: absolute;
  width: 49px;
  height: 49px;
  top: 250px;
  line-height: 40px;
  border-radius: 50%;
  border: 1px solid $theme-color;
  color: $theme-color;
  font-size: 30px;
  font-weight: bold;
  text-align: center;
  cursor: pointer;
}
.prev {
  left: -65px;
}
.next {
  right: -65px;
}
</style>