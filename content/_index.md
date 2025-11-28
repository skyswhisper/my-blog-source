<!-- ================== START: 最终版-暗黑流体切片特效 (修复版) ================== -->

<!-- 1. 底层背景 -->
<div id="static-bg" style="
    position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -2;
    background-image: url('/images/bg.jpg'); 
    background-size: cover;
    background-position: center;
    filter: brightness(1.0) blur(5px);
    transform: scale(1.1);
"></div>

<!-- 2. 顶层画布 -->
<canvas id="image-grid" style="position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1; pointer-events: none;"></canvas>

<script>
(function() {
    const canvas = document.getElementById('image-grid');
    const ctx = canvas.getContext('2d');
    let width, height;
    let particles = [];
    
    // ============ 【配置区】 ============
    const CONFIG = {
        imgSrc: '/images/bg.jpg', // 【关键】必须和上面的背景图一致
        
        // 切片形状
        cols: 10,             // 列数 (越多越细腻)
        rows: 30,             // 行数
        
        // 椭圆力场
        mouseRadiusX: 400,    // 宽范围
        mouseRadiusY: 120,    // 窄范围
        
        // 物理手感
        dragStrength: 1.75,    // 拖拽总力度
        dragRatioY: 0.15,      // 【关键修改】竖直力度系数 (0.1代表竖直力只有水平力的10%)
        
        stiffness: 0.15,      // 回弹硬度
        damping: 0.4         // 摩擦力 (0.93 非常顺滑)
    };
    // ===================================

    let mouse = { x: -1000, y: -1000, vx: 0, vy: 0 };
    let image = new Image();
    image.src = CONFIG.imgSrc;
    
    image.onload = () => {
        init();
        animate();
    };

    class Particle {
        // 【修改 1】构造函数增加 sw, sh (Source Width/Height)
        constructor(x, y, w, h, u, v, sw, sh) {
            this.originX = x; 
            this.originY = y;
            this.x = x;       
            this.y = y;
            this.w = w;       // 屏幕显示的宽
            this.h = h;       // 屏幕显示的高
            
            this.u = u;       // 原图剪切位置 X
            this.v = v;       // 原图剪切位置 Y
            this.sw = sw;     // 【新增】原图剪切宽度
            this.sh = sh;     // 【新增】原图剪切高度
            
            this.vx = 0;
            this.vy = 0;
        }

        update() {
            let dx = mouse.x - this.x;
            let dy = mouse.y - this.y;
            
            let normalizedDistSq = (dx * dx) / (CONFIG.mouseRadiusX * CONFIG.mouseRadiusX) + 
                                   (dy * dy) / (CONFIG.mouseRadiusY * CONFIG.mouseRadiusY);

            if (normalizedDistSq < 1) {
                let normDist = Math.sqrt(normalizedDistSq);
                let forceFactor = (1 - normDist);

                this.vx += mouse.vx * forceFactor * CONFIG.dragStrength;
                this.vy += mouse.vy * forceFactor * CONFIG.dragStrength * CONFIG.dragRatioY;
            }

            let springDx = this.originX - this.x;
            let springDy = this.originY - this.y;

            this.vx += springDx * CONFIG.stiffness;
            this.vy += springDy * CONFIG.stiffness;

            this.vx *= CONFIG.damping;
            this.vy *= CONFIG.damping;

            this.x += this.vx;
            this.y += this.vy;
        }

        draw() {
            // 【修改 2】drawImage 使用计算好的 sw 和 sh
            ctx.drawImage(
                image, 
                this.u, this.v,     // 原图起点
                this.sw, this.sh,   // 【修复】原图剪切尺寸 (修正了撕裂问题)
                this.x, this.y,     // 屏幕位置
                this.w + 1.0, this.h + 1.0 // 屏幕尺寸 (保留+1防止缝隙)
            );
        }
    }

    function init() {
        width = canvas.width = window.innerWidth;
        height = canvas.height = window.innerHeight;
        particles = [];

        const tileW = width / CONFIG.cols;
        const tileH = height / CONFIG.rows;
        
        // 计算 Cover 比例
        const scale = Math.max(width / image.width, height / image.height);
        const offsetX = (width - image.width * scale) / 2;
        const offsetY = (height - image.height * scale) / 2;

        // 【修改 3】预先计算原图上的采样切片大小
        // 因为屏幕上的 tileW 是缩放后的，所以原图上的尺寸要除以 scale
        const sourceChunkW = tileW / scale;
        const sourceChunkH = tileH / scale;

        for (let i = 0; i < CONFIG.cols; i++) {
            for (let j = 0; j < CONFIG.rows; j++) {
                let x = i * tileW;
                let y = j * tileH;
                
                // 纹理映射
                let u = (x - offsetX) / scale;
                let v = (y - offsetY) / scale;

                // 【修改 4】传入 sourceChunkW 和 sourceChunkH
                particles.push(new Particle(x, y, tileW, tileH, u, v, sourceChunkW, sourceChunkH));
            }
        }
    }

    function animate() {
        ctx.clearRect(0, 0, width, height);
        particles.forEach(p => {
            p.update();
            p.draw();
        });
        mouse.vx = 0;
        mouse.vy = 0;
        requestAnimationFrame(animate);
    }

    window.addEventListener('resize', init);
    
    window.addEventListener('mousemove', e => {
        mouse.x = e.clientX;
        mouse.y = e.clientY;
        mouse.vx = e.movementX || 0; 
        mouse.vy = e.movementY || 0;
    });

    window.addEventListener('mouseleave', () => {
        mouse.x = -1000;
        mouse.y = -1000;
    });

})();
</script>

<!-- ... 上面的流体背景特效代码千万别动 ... -->
<!-- ... 从这里开始替换下半部分 ... -->

<!-- 1. 副标题 (初始是隐藏的，等脚本移动位置后再显示) -->
<!-- 调整 font-size: 2.5rem 就可以改变大小 -->
<h2 id="my-subtitle" style="
    color: #ffffff !important; 
    text-align: center; 
    font-size: 4rem;   /* <--- 这里调整字体大小！(原先可能是1.5) */
    font-weight: bold;   /* 加粗 */
    margin: 0.9rem 1;      /* 上下间距 */
    min-height: 0em; 
    text-shadow: 0 2px 2px rgba(0, 0, 0, 0.6); /* 加重阴影，防止看不清 */
    border: none !important;
    display: none;       /* 先隐藏，移位成功后再显示，防止闪烁 */
"></h2>

<!-- 2. 按钮容器 -->
<div class="home-button-container" style="position: relative; z-index: 10;">
  <a href="/posts/" class="enter-btn">
    <i class="fa-solid fa-paper-plane"></i> 立即启程
  </a>
</div>

<!-- 3. 引入 TypeIt 库 -->
<script src="https://unpkg.com/typeit@8.7.1/dist/index.umd.js"></script>

<!-- 4. 魔法脚本：移位 + 打字 -->
<script>
  document.addEventListener('DOMContentLoaded', () => {
    // --- 第一步：移花接木 (把副标题挪到社交图标上面) ---
    const subtitle = document.getElementById('my-subtitle');
    const socialLinks = document.querySelector('.home-profile .links'); // 找到社交图标栏
    const profile = document.querySelector('.home-profile'); // 找到个人档案大框
    
    if (subtitle && socialLinks && socialLinks.parentNode) {
      // 如果找到了社交图标，就把副标题插在它前面
      socialLinks.parentNode.insertBefore(subtitle, socialLinks);
      subtitle.style.display = "block"; // 移位成功，显示出来
    } else if (profile) {
      // 如果没找到社交图标（万一你关了），就插在 profile 的最后
      profile.appendChild(subtitle);
      subtitle.style.display = "block";
    } else {
      // 实在找不到家，就原地显示
      subtitle.style.display = "block";
    }

    // --- 第二步：开始打字 ---
    new TypeIt("#my-subtitle", {
      speed: 100,       
      lifeLike: true,
      cursor: true,
      cursorChar: "|",
      loop: false, 
    })
    .type("表达，")       
    .pause(200)          // 停顿 0.2 秒
    .type("为了无法表达的共鸣 ") 
    .go();
  });
</script>