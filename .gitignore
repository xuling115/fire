(function ($) {

    var fire = function (element, options) {
        this.element = element;
        this.options = options;
    };
    fire.prototype = {       
        init: function () {
            this.cas = this.element;
            this.ctx = this.cas.getContext("2d")   
            this.cas.width = this.options.width;
            this.cas.height = this.options.height;
            var r=0;
            for (var i=0; i<this.options.count; i++){
                r = random(3,8);
                Fireflies.push(new Fireflie({
                    cas: this.cas,
                    r: r,
                    x: random(r, this.cas.width - r),
                    y: random(r, this.cas.height - r),
                    vx: random(-5,5),
                    vy: random(-5, 5)
                }));
            }   
        },
        //更新
        update: function () {
            this.ctx.clearRect(0,0,this.cas.width,this.cas.height);                
            for (var i = 0; i < Fireflies.length; i++) {
                Fireflies[i].move();
            }
        },
        //循环
        loop: function () {
            var _this = this;
            this.update();
            RAF(function () {
                _this.loop();
            })
        },
        //开始
        start: function () {
            this.init();               
            this.loop();
        }
    }


    $.fn.fire = function (options) {
        // 合并参数
        return this.each(function () {
            var opts = $.extend(true, {}, $.fn.fire.defaults, typeof options === "object" ? options : {});
            // 在这里编写相应的代码进行处理
           new fire(this, opts).start();

        });
    };

    $.fn.fire.defaults = {
        count: 30,
        width: 1000,
        height: 600
    }



   //动画兼容
    window.RAF = (function () {
        return window.requestAnimationFrame || window.webkitRequestAnimationFrame || window.mozRequestAnimationFrame || window.oRequestAnimationFrame || window.msRequestAnimationFrame || function (callback) { window.setTimeout(callback, 1000 / 60); };
    })();
    var PI2 = Math.PI * 2;
    function random(min, max) {
        if (Object.prototype.toString.call(min) == "[object Array]")
            return min[~~(Math.random() * min.length)];
        if (!(typeof max == "number"))
            max = min || 1, min = 0;
        return min + Math.random() * (max - min);
    };

     //萤火虫        
    var Fireflies = [];
    var Fireflie = function (cf) {
        //设置参数
        this.r= cf.r;
        this.x = cf.x;
        this.y = cf.y;
        this.vx = cf.vx;
        this.vy = cf.vy;
        
        //速度阻力
        this.drag = 0.62;
        //角度偏离参数
        this.wander = 0.12;
        //偏离角度
        this.theta = random(PI2);
        this.cas = cf.cas;
    };

    Fireflie.prototype = {
        //绘制
        paint: function () {
            var colorRgba = ["RGBA(233,244,35,1)","RGBA(48,225,222,1)"];                
            ctx = this.cas.getContext("2d")
            var ag = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, this.r);
            var colorIndex = Math.floor(Math.random()*2)
            ag.addColorStop(0.0, colorRgba[colorIndex]);
            ag.addColorStop(0.2, colorRgba[colorIndex]);
            ag.addColorStop(0.34, colorRgba[colorIndex].replace(",1)", ",0.2)"));
            ag.addColorStop(1.0, colorRgba[colorIndex].replace(",1)", ",0.0)"));
            ctx.beginPath();
            ctx.fillStyle = ag;
            ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
            ctx.fill();
           
        },
        //运动
        move: function () { 
            this.vx *=this.drag;
            this.vy *=this.drag;
            this.x += this.vx;
            this.y += this.vy;
            this.theta += random(-0.5, 0.5) * this.wander;                
            this.vx +=  Math.cos(this.theta) * 0.1;
            this.vy +=  Math.sin(this.theta) * 0.1;
            if ((this.x + this.r) >= this.cas.width || (this.x - this.r) <= 0) {
                this.vx *= -1;
                this.theta = random(PI2);
            }
            if ((this.y + this.r) >= this.cas.height || (this.y - this.r) <= 0) {
                this.vy *= -1;
                this.theta = random(PI2);
            }             
            this.paint();
        }
    };
})(jQuery);

    
      
       
