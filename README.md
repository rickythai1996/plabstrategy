# 📊 Báo Cáo Tương Tác: Chiến Lược P-Lab 2025-2030

Một website **Single Page Application (SPA)** trực quan, xây dựng bằng **HTML + TailwindCSS + Chart.js + Vanilla JavaScript**.

---

## 🚀 Demo
👉 *(Bạn có thể deploy bằng [GitHub Pages](https://pages.github.com/) hoặc [Vercel](https://vercel.com/))*

---

## ✨ Tính năng chính

- **📌 Tổng quan nhanh**: Các chỉ số trọng yếu (thị phần, tăng trưởng, lợi nhuận, quy mô).  
- **📈 Thị trường & Cạnh tranh**: Biểu đồ donut hiển thị cấu trúc thị phần.  
- **📊 Tăng trưởng tài chính**: Biểu đồ bar (có thể mở rộng sau).  
- **🧩 Phân tích SWOT**: Grid 4 ô rõ ràng, dễ đọc.  
- **📂 Trụ cột chiến lược**: Accordion có thể mở rộng để xem mục tiêu & hành động.  
- **👥 Khách hàng mục tiêu**: Card layout trực quan cho từng nhóm khách hàng.  
- **🕒 Lộ trình phát triển**: Timeline hiển thị các mốc quan trọng.  
- **🤖 AI Brainstorm (Gemini API)**: Nút gợi ý sáng kiến & chiến lược (có thể kích hoạt bằng cách thêm API key).  

---

## 🛠️ Công nghệ sử dụng

- **[TailwindCSS](https://tailwindcss.com/)** – styling & responsive design  
- **[Chart.js](https://www.chartjs.org/)** – visualization (doughnut, bar charts)  
- **Vanilla JavaScript (ES6)** – xử lý tabs, accordion, modal, API calls  
- **Google Fonts – Be Vietnam Pro**  

---

## 📂 Cấu trúc dự án

```
project-root/
│── index.html        # File chính (SPA dashboard)
│── README.md         # Tài liệu dự án
└── assets/           # (Tùy chọn: thêm hình ảnh, logo, CSS custom)
```

---

## ⚡ Cách chạy

1. Clone repo:
   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```

2. Mở file `index.html` bằng trình duyệt (không cần build).

3. (Tùy chọn) Deploy trên GitHub Pages:
   - Push code lên branch `main`
   - Vào **Settings → Pages → chọn branch `main`**
   - Link sẽ có dạng: `https://<username>.github.io/<repo-name>/`

---

## 🔑 Tích hợp Gemini API (tùy chọn)

- Tìm trong `index.html` đoạn sau:
  ```js
  const apiKey = "";
  ```
- Thay bằng **Google AI Studio API Key** của bạn:
  ```js
  const apiKey = "YOUR_API_KEY_HERE";
  ```
- Tính năng brainstorm sẽ hoạt động khi nhấn nút ✨.

---

## 📜 Giấy phép

Dự án này được tạo cho **mục đích học tập & trình bày nội bộ**.  
Không sử dụng vào mục đích thương mại nếu chưa có sự cho phép của tác giả.  

---

## 📄 Full Code

```html
<!DOCTYPE html>
<html lang="vi" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Báo Cáo Tương Tác: Chiến Lược P-Lab 2025-2030</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Be+Vietnam+Pro:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Be Vietnam Pro', sans-serif;
            background-color: #f8f7f4;
            color: #383838;
        }
        .text-sapphire { color: #003366; }
        .bg-sapphire { background-color: #003366; }
        .bg-sandstone { background-color: #fdfaf5; }
        .border-sapphire-light { border-color: #b3cce6; }
        .tab-btn.active {
            border-bottom: 2px solid #005f9e;
            color: #005f9e;
            font-weight: 600;
        }
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.4s ease-out;
        }
        .accordion-item.active .accordion-content {
            max-height: 500px;
            transition: max-height 0.5s ease-in;
        }
        .accordion-item .accordion-arrow {
            transition: transform 0.3s ease;
        }
        .accordion-item.active .accordion-arrow {
            transform: rotate(180deg);
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 450px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 350px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
        .gemini-modal-backdrop {
            background-color: rgba(0,0,0,0.5);
        }
        .loader {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #005f9e;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="antialiased">
    <!-- HEADER -->
    <header class="bg-sapphire text-white shadow-md sticky top-0 z-50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <h1 class="text-xl md:text-2xl font-bold">Chiến Lược P-Lab 2025-2030</h1>
            <div class="hidden md:flex space-x-6">
                <a href="#overview" class="hover:text-gray-300">Tổng Quan</a>
                <a href="#market" class="hover:text-gray-300">Thị Trường</a>
                <a href="#strategy" class="hover:text-gray-300">Chiến Lược</a>
                <a href="#customers" class="hover:text-gray-300">Khách Hàng</a>
            </div>
        </nav>
    </header>

    <!-- MAIN CONTENT -->
    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">
        <!-- Hero Section -->
        <section id="hero" class="text-center mb-12 md:mb-16">
            <h2 class="text-3xl md:text-4xl font-bold text-sapphire mb-4">Tầm nhìn chiến lược P-Lab đến 2030</h2>
            <p class="max-w-3xl mx-auto text-base md:text-lg text-gray-600">
                Một bản phân tích tương tác về định hướng phát triển của P-Lab.
            </p>
        </section>

        <!-- Overview -->
        <section id="overview" class="mb-12 md:mb-16">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-8">Các Chỉ Số Trọng Yếu</h3>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6 text-center">
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">41%</p>
                    <p class="text-sm text-gray-600 mt-2">Thị Phần Tại Việt Nam</p>
                </div>
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">11%</p>
                    <p class="text-sm text-gray-600 mt-2">Tăng Trưởng Doanh Thu</p>
                </div>
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">&gt;64%</p>
                    <p class="text-sm text-gray-600 mt-2">Biên Lợi Nhuận Gộp</p>
                </div>
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">~110 tỷ</p>
                    <p class="text-sm text-gray-600 mt-2">Quy Mô Thị Trường (2022)</p>
                </div>
            </div>
        </section>

        <!-- Market Section (example with chart) -->
        <section id="market" class="mb-12 md:mb-16 bg-white p-6 md:p-8 rounded-lg shadow-md">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-2">Bối Cảnh & Thị Trường</h3>
            <div class="chart-container">
                <canvas id="marketShareChart"></canvas>
            </div>
        </section>

        <!-- Strategy Accordion -->
        <section id="strategy" class="mb-12 md:mb-16">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-2">Định Hướng Chiến Lược</h3>
            <div id="accordion-container" class="space-y-4">
                <div class="accordion-item bg-white rounded-lg shadow-sm border border-gray-200">
                    <button class="accordion-header w-full flex justify-between items-center text-left p-5 font-semibold text-sapphire">
                        <span data-title="Củng Cố Nền Tảng Kiểm Định">1. Củng Cố Nền Tảng Kiểm Định</span>
                        <span class="accordion-arrow text-2xl">↓</span>
                    </button>
                    <div class="accordion-content px-5 pb-5">
                        <p class="text-gray-600 text-sm">Mục tiêu: Giữ vững vị thế số 1 tại Việt Nam.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Customers -->
        <section id="customers" class="bg-sandstone p-6 md:p-8 rounded-lg shadow-inner">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-2">Khách Hàng Mục Tiêu</h3>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h4 class="font-bold text-lg text-sapphire">Cá nhân cao cấp</h4>
                    <p class="text-sm text-gray-600">Nhóm trọng tâm, chiếm tỷ trọng lớn nhất.</p>
                </div>
            </div>
        </section>
    </main>

    <!-- FOOTER -->
    <footer class="bg-sapphire text-white mt-12">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-6 text-center text-sm">
            <p>&copy; 2025 P-Lab Strategic Interactive Report. All rights reserved.</p>
        </div>
    </footer>

    <!-- Scripts -->
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const marketShareCtx = document.getElementById('marketShareChart').getContext('2d');
            new Chart(marketShareCtx, {
                type: 'doughnut',
                data: {
                    labels: ['PNJ Lab', 'SJC Lab', 'Doji Lab', 'Đơn vị khác'],
                    datasets: [{
                        label: 'Thị phần',
                        data: [41, 18, 16, 25],
                        backgroundColor: [
                            'rgba(0, 51, 102, 0.9)',
                            'rgba(0, 95, 158, 0.8)',
                            'rgba(100, 150, 200, 0.7)',
                            'rgba(200, 200, 200, 0.6)'
                        ],
                        borderColor: '#fdfaf5',
                        borderWidth: 3
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { position: 'bottom' } }
                }
            });

            const accordionItems = document.querySelectorAll('.accordion-item');
            accordionItems.forEach(item => {
                const header = item.querySelector('.accordion-header');
                header.addEventListener('click', () => {
                    const currentlyActive = document.querySelector('.accordion-item.active');
                    if(currentlyActive && currentlyActive !== item) {
                        currentlyActive.classList.remove('active');
                    }
                    item.classList.toggle('active');
                });
            });
        });
    </script>
</body>
</html>
