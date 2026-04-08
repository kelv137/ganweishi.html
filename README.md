<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>柑为食·肝维视 - AI肝脏健康管理平台</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', 'Roboto', 'Noto Sans', system-ui, -apple-system, sans-serif;
            background-color: #fef9f0;   /* 暖白基底 */
            color: #2c3e2f;
            line-height: 1.5;
        }

        /* 背景图（纯CSS实现：柔和的绿橙渐变 + 模糊叶片/光晕）完全符合描述，不抢文字 */
        .page-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -2;
            background: linear-gradient(135deg, 
                #eaf7ea 0%,      /* 淡绿色 */
                #f3f0e0 50%,
                #fff1e0 100%);   /* 淡米橙色 */
        }

        /* 模糊叶片装饰（左侧） */
        .bg-leaf {
            position: fixed;
            left: -5%;
            top: 15%;
            width: 35%;
            height: 45%;
            background: radial-gradient(circle, rgba(140, 180, 120, 0.15) 0%, rgba(100, 140, 80, 0.05) 80%);
            filter: blur(45px);
            border-radius: 60% 40% 50% 50%;
            z-index: -1;
            pointer-events: none;
        }

        /* 模糊柑橘光晕（右侧） */
        .bg-orange {
            position: fixed;
            right: -5%;
            bottom: 10%;
            width: 40%;
            height: 50%;
            background: radial-gradient(ellipse, rgba(255, 180, 100, 0.18) 0%, rgba(255, 140, 70, 0.05) 70%);
            filter: blur(50px);
            border-radius: 50% 40% 60% 50%;
            z-index: -1;
            pointer-events: none;
        }

        /* 额外柔光过渡 */
        .bg-transition {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(ellipse at 30% 40%, rgba(255,250,235,0.3) 0%, rgba(245,245,225,0.1) 100%);
            z-index: -1;
            pointer-events: none;
        }

        .container {
            max-width: 1280px;
            margin: 0 auto;
            padding: 0 24px;
            position: relative;
            z-index: 2;
        }

        /* 顶部导航栏 */
        .navbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 24px 0 20px;
            flex-wrap: wrap;
            border-bottom: 1px solid rgba(80, 110, 70, 0.15);
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 1.7rem;
            font-weight: 600;
            color: #2a6b3c;
        }
        .logo-icon {
            width: 42px;
            height: 42px;
            background: #ff9142;
            border-radius: 30px 30px 30px 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
        }
        .logo-text {
            letter-spacing: 1px;
        }
        .logo small {
            font-size: 0.8rem;
            font-weight: normal;
            color: #e67e22;
            margin-left: 5px;
        }

        .nav-links {
            display: flex;
            gap: 28px;
            align-items: center;
        }
        .nav-links a {
            text-decoration: none;
            color: #2d4a2c;
            font-weight: 500;
            transition: 0.2s;
        }
        .nav-links a:hover {
            color: #e67e22;
        }
        .btn-outline {
            border: 1.5px solid #2e8b57;
            padding: 8px 18px;
            border-radius: 40px;
            background: transparent;
            color: #2e8b57;
        }
        .btn-outline:hover {
            background: #2e8b5710;
        }

        /* Hero 区 */
        .hero {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            padding: 60px 0 40px;
            gap: 30px;
        }
        .hero-left {
            flex: 1;
            min-width: 260px;
        }
        .hero-left h1 {
            font-size: 3rem;
            font-weight: 700;
            background: linear-gradient(135deg, #2a6b3c, #ff9142);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            line-height: 1.2;
            margin-bottom: 20px;
        }
        .hero-tag {
            color: #e67e22;
            font-size: 1.3rem;
            font-weight: 500;
            margin-bottom: 30px;
            border-left: 4px solid #ff9142;
            padding-left: 16px;
        }
        .hero-buttons {
            display: flex;
            gap: 16px;
            flex-wrap: wrap;
        }
        .btn-primary-green {
            background-color: #2e8b57;
            border: none;
            padding: 12px 28px;
            border-radius: 40px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            font-size: 1rem;
            transition: 0.2s;
        }
        .btn-primary-green:hover {
            background-color: #236b45;
        }
        .btn-primary-orange {
            background-color: #ff9142;
            border: none;
            padding: 12px 28px;
            border-radius: 40px;
            color: white;
            font-weight: bold;
            cursor: pointer;
        }
        .btn-primary-orange:hover {
            background-color: #e67e22;
        }
        .hero-right {
            flex: 0.8;
            text-align: center;
        }
        .hero-badge {
            background: rgba(255,255,245,0.7);
            backdrop-filter: blur(4px);
            border-radius: 60px;
            padding: 12px 20px;
            display: inline-block;
            font-size: 0.9rem;
            color: #2e5a32;
        }

        /* 三大模块卡片区 */
        .section-title {
            text-align: center;
            font-size: 2rem;
            margin: 50px 0 20px;
            color: #2a6b3c;
            font-weight: 600;
        }
        .cards {
            display: flex;
            flex-wrap: wrap;
            gap: 30px;
            margin: 30px 0 40px;
            justify-content: center;
        }
        .card {
            flex: 1;
            min-width: 280px;
            background: rgba(255, 255, 250, 0.9);
            backdrop-filter: blur(2px);
            border-radius: 32px;
            padding: 28px 22px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.03), 0 2px 5px rgba(0,0,0,0.05);
            transition: all 0.25s ease;
            border: 1px solid rgba(100, 130, 80, 0.2);
        }
        .card:hover {
            transform: translateY(-6px);
            box-shadow: 0 20px 30px -12px rgba(0,0,0,0.1);
            border-color: #ff914280;
        }
        .card-icon {
            font-size: 48px;
            margin-bottom: 18px;
        }
        .card h3 {
            font-size: 1.6rem;
            margin-bottom: 12px;
            color: #2c5e2f;
        }
        .card .price {
            font-weight: bold;
            color: #e67e22;
            margin: 12px 0;
            font-size: 1.3rem;
        }
        .feature-list {
            margin: 15px 0;
            padding-left: 20px;
            color: #445c3e;
        }
        .feature-list li {
            margin: 6px 0;
        }
        .card-footer {
            margin-top: 20px;
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }
        .btn-card-green {
            background: #2e8b57;
            color: white;
            border: none;
            padding: 10px 18px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: 500;
        }
        .btn-card-orange {
            background: #ff9142;
            color: white;
            border: none;
            padding: 10px 18px;
            border-radius: 40px;
            cursor: pointer;
        }
        .btn-outline-card {
            background: transparent;
            border: 1px solid #2e8b57;
            padding: 9px 17px;
            border-radius: 40px;
            color: #2e8b57;
            cursor: pointer;
        }
        .small-note {
            font-size: 0.7rem;
            color: #8f9e8b;
            margin-top: 15px;
            text-align: center;
        }

        /* 橙色专属功能区 */
        .doctor-banner {
            background: #ff9142;
            border-radius: 28px;
            margin: 40px 0 30px;
            padding: 28px 30px;
            color: white;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 15px;
        }
        .doctor-banner p {
            font-size: 1.2rem;
            font-weight: 500;
        }
        .doctor-banner button {
            background: white;
            border: none;
            padding: 10px 30px;
            border-radius: 40px;
            color: #e67e22;
            font-weight: bold;
            cursor: pointer;
        }

        /* 底部合规与信任 */
        .tech-strip {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 35px;
            margin: 40px 0 30px;
            padding: 20px 0;
            border-top: 1px solid #e2e6dd;
            border-bottom: 1px solid #e2e6dd;
        }
        .tech-item {
            display: flex;
            align-items: center;
            gap: 8px;
            color: #446644;
        }
        .footer-links {
            text-align: center;
            margin: 25px 0 40px;
            font-size: 0.8rem;
            color: #6f7c68;
        }
        .footer-links a {
            margin: 0 12px;
            color: #587a50;
            text-decoration: none;
        }
        .security-badge {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(46,139,87,0.9);
            backdrop-filter: blur(8px);
            padding: 8px 16px;
            border-radius: 40px;
            color: white;
            font-size: 0.7rem;
            display: flex;
            align-items: center;
            gap: 6px;
            z-index: 99;
        }

        @media (max-width: 750px) {
            .navbar {
                flex-direction: column;
                gap: 15px;
            }
            .hero-left h1 {
                font-size: 2.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="page-bg"></div>
    <div class="bg-leaf"></div>
    <div class="bg-orange"></div>
    <div class="bg-transition"></div>

    <div class="container">
        <!-- 导航栏 -->
        <div class="navbar">
            <div class="logo">
                <div class="logo-icon">🍊</div>
                <div class="logo-text">柑为食·肝维视<small> | 精准评估 科学养护</small></div>
            </div>
            <div class="nav-links">
                <a href="#">AI病理分析</a>
                <a href="#">柑轻素</a>
                <a href="#">健康数据</a>
                <a href="#">关于我们</a>
                <a href="#" class="btn-outline">登录/注册</a>
                <a href="#" class="btn-outline">企业入口</a>
            </div>
        </div>

        <!-- Hero 首屏 -->
        <div class="hero">
            <div class="hero-left">
                <h1>柑为食，肝维视<br>您的AI肝脏健康伙伴</h1>
                <div class="hero-tag">精准评估 · 科学养护 · 全程管理</div>
                <div class="hero-buttons">
                    <button class="btn-primary-green" onclick="alert('AI分析平台即将开放，可联系客服申请试用')">开始AI分析</button>
                    <button class="btn-primary-orange" onclick="alert('前往商城：柑轻素功能性食品 198元/盒')">购买柑轻素</button>
                </div>
            </div>
            <div class="hero-right">
                <div class="hero-badge">🐾 动物实验功效依据 | 普通食品不替代药物</div>
            </div>
        </div>

        <!-- 三大核心模块 -->
        <div class="section-title">三大核心服务 · 闭环管理</div>
        <div class="cards">
            <!-- 模块1: AI病理分析 -->
            <div class="card">
                <div class="card-icon">🔬🧠</div>
                <h3>肝智评 · AI病理分析</h3>
                <div class="feature-list">
                    <li>✅ 脂肪空泡自动分割（Dice≥0.90）</li>
                    <li>✅ 全球首创1区/3区差异定量</li>
                    <li>✅ 批量处理 + PDF/Excel报告</li>
                    <li>💰 单张基础分析 200元/张</li>
                    <li>📦 完整药效服务包 5000元/组</li>
                </div>
                <div class="card-footer">
                    <button class="btn-card-green" onclick="alert('免费试用3张切片联系客服: trial@ganweishi.com')">免费试用3张</button>
                    <button class="btn-outline-card" onclick="alert('进入平台: ai.ganweishi.com  (演示)')">进入平台</button>
                </div>
                <div class="small-note">*仅限科研使用，不用于临床诊断</div>
            </div>

            <!-- 模块2: 普通食品（功能性定位） -->
            <div class="card">
                <div class="card-icon">🍊💊</div>
                <h3>柑轻素™ 固体饮料</h3>
                <div class="price">¥198/盒 | 体验装 ¥68/10袋</div>
                <div class="feature-list">
                    <li>✔ 每袋≥50mg 川陈皮素</li>
                    <li>✔ 水飞蓟提取物+VE+益生元</li>
                    <li>✔ 执行GB/T 29602（普通食品）</li>
                    <li>📊 动物实验数据支持AMPK通路</li>
                </div>
                <div class="card-footer">
                    <button class="btn-card-orange" onclick="alert('跳转官方商城购买柑轻素，新用户享包邮')">立即购买</button>
                    <button class="btn-outline-card" onclick="alert('了解更多：产品成分及研究进展')">了解更多</button>
                </div>
                <div class="small-note">本品为普通食品，不能替代药物。动物实验数据详见3.2.5</div>
            </div>

            <!-- 模块3: 数据反馈与效果记录 -->
            <div class="card">
                <div class="card-icon">📈🔄</div>
                <h3>数据闭环 · 效果记录</h3>
                <div class="feature-list">
                    <li>📤 上传前后B超/弹性扫描报告</li>
                    <li>📊 AI生成个人健康趋势图</li>
                    <li>🔒 数据加密，30天自动删除</li>
                    <li>👥 B端组间差异分析一键导出</li>
                </div>
                <div class="card-footer">
                    <button class="btn-card-green" onclick="alert('请登录后上传报告，仅用于健康趋势参考')">上传报告</button>
                    <button class="btn-outline-card" onclick="alert('查看示例报告：模拟数据演示')">查看示例</button>
                </div>
                <div class="small-note">数据所有权归您，不作他用</div>
            </div>
        </div>

        <!-- 医务工作者专享横幅 -->
        <div class="doctor-banner">
            <p>👩‍⚕️ 医务工作者专享：凭执业证注册“医者护航” → AI分析永久免费，产品享7折</p>
            <button onclick="alert('认证入口即将开放，请准备好执业证照片')">立即认证</button>
        </div>

        <!-- 技术支撑与合规区 -->
        <div class="tech-strip">
            <div class="tech-item">🧩 QuPath 开源平台</div>
            <div class="tech-item">🤖 深度学习分割模型</div>
            <div class="tech-item">📄 GB/T 29602 标准</div>
            <div class="tech-item">🏭 SC认证代工厂</div>
        </div>

        <div class="footer-links">
            <a href="#">服务协议</a> |
            <a href="#">隐私政策</a> |
            <a href="#">消费者提示：普通食品不能替代药物</a> |
            <a href="#">联系我们</a>
        </div>
        <div style="text-align:center; font-size:0.7rem; color:#8ba07e; margin-bottom: 30px;">
            © 2026 柑为食·肝维视 团队 | 本网站仅提供健康信息参考，不做医疗诊断依据
        </div>
    </div>

    <!-- 右下角安全锁 -->
    <div class="security-badge">
        🔒 数据加密传输 · 等保二级认证
    </div>

    <script>
        // 简单交互：所有按钮alert仅为演示，真实网站可替换为实际链接
        console.log("柑为食·肝维视官网已加载——绿色+橙色风格，三模块完整");
    </script>
</body>
</html>
