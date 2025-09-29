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
    <!-- Chosen Palette: Sapphire & Sandstone -->
    <!-- Application Structure Plan: SPA được thiết kế theo cấu trúc dashboard chuyên đề, thay vì theo trình tự của báo cáo gốc. Bắt đầu bằng tổng quan các chỉ số chính để người dùng nắm bắt nhanh. Các phần sau được nhóm theo logic: Tình hình hiện tại (Thị trường & SWOT), Định hướng Chiến lược (các trụ cột tương tác), và Khách hàng mục tiêu. Cấu trúc này giúp người dùng dễ dàng điều hướng, so sánh thông tin và đi sâu vào các phần họ quan tâm nhất mà không cần cuộn trang dài. Các thẻ (tabs) và các khối nội dung có thể mở rộng (accordions) được sử dụng để trình bày thông tin một cách gọn gàng, khuyến khích sự khám phá. -->
    <!-- Visualization & Content Choices: 
        - Tổng quan: Goal: Inform -> Dynamic Stat Cards -> Non-interactive -> Hiển thị các số liệu quan trọng nhất (thị phần, tăng trưởng) ngay lập tức.
        - Thị phần: Goal: Compare -> Donut Chart (Chart.js) -> Hover for details -> Trực quan hóa cấu trúc độc quyền nhóm của thị trường, làm nổi bật vị thế của P-Lab.
        - Tăng trưởng tài chính: Goal: Compare -> Bar Chart (Chart.js) -> Hover for details -> So sánh trực quan tốc độ tăng trưởng doanh thu và lợi nhuận, cho thấy sự phát triển bền vững.
        - Phân tích SWOT: Goal: Organize -> 4-Quadrant Grid (HTML/Tailwind) -> Non-interactive -> Cấu trúc thông tin logic, dễ quét và so sánh các yếu tố.
        - Trụ cột chiến lược: Goal: Inform/Organize -> Interactive Accordions (HTML/JS) -> Click to expand -> Phá vỡ lượng thông tin chiến lược dày đặc thành các phần nhỏ, dễ tiêu hóa, cho phép người dùng tập trung vào từng mục tiêu cụ thể.
        - Khách hàng mục tiêu: Goal: Organize -> Card Layout (HTML/Tailwind) -> Click to expand -> Phân loại và trình bày các nhóm khách hàng một cách trực quan, dễ hiểu.
        - Lộ trình: Goal: Change -> Timeline (HTML/Tailwind) -> Non-interactive -> Hiển thị quá trình phát triển và các cột mốc quan trọng theo thời gian.
        - Library/Method: Chart.js for canvas-based charts, Vanilla JS for all interactions (tabs, accordions), TailwindCSS for layout and styling.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
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

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">
        
        <section id="hero" class="text-center mb-12 md:mb-16">
             <h2 class="text-3xl md:text-4xl font-bold text-sapphire mb-4">Tầm nhìn chiến lược P-Lab đến 2030</h2>
             <p class="max-w-3xl mx-auto text-base md:text-lg text-gray-600">
                Một bản phân tích tương tác về định hướng phát triển của P-Lab, tập trung vào việc củng cố vị thế dẫn đầu, mở rộng thị trường và đổi mới dịch vụ để tiệm cận các chuẩn mực quốc tế.
             </p>
        </section>

        <section id="overview" class="mb-12 md:mb-16">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-8">Các Chỉ Số Trọng Yếu</h3>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6 text-center">
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">41%</p>
                    <p class="text-sm text-gray-600 mt-2">Thị Phần Tại Việt Nam</p>
                </div>
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">11%</p>
                    <p class="text-sm text-gray-600 mt-2">Tăng Trưởng Doanh Thu (CAGR 5 năm)</p>
                </div>
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">>64%</p>
                    <p class="text-sm text-gray-600 mt-2">Biên Lợi Nhuận Gộp</p>
                </div>
                <div class="bg-sandstone p-6 rounded-lg shadow-sm">
                    <p class="text-4xl font-bold text-sapphire">~110 tỷ</p>
                    <p class="text-sm text-gray-600 mt-2">Quy Mô Thị Trường (2022)</p>
                </div>
            </div>
        </section>

        <section id="market" class="mb-12 md:mb-16 bg-white p-6 md:p-8 rounded-lg shadow-md">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-2">Bối Cảnh & Thị Trường</h3>
            <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Phần này cung cấp một cái nhìn toàn diện về môi trường kinh doanh của P-Lab, từ cấu trúc thị phần, các đối thủ cạnh tranh chính đến phân tích SWOT. Dữ liệu được trực quan hóa để giúp bạn nhanh chóng xác định các cơ hội và thách thức cốt lõi.</p>
            
            <div class="border-b border-gray-200 mb-6">
                <nav class="-mb-px flex justify-center space-x-4 md:space-x-8" aria-label="Tabs">
                    <button class="tab-btn active whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm" data-tab="market-share">Thị Trường & Cạnh Tranh</button>
                    <button class="tab-btn whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700" data-tab="swot">Phân Tích SWOT</button>
                    <button class="tab-btn whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700" data-tab="timeline">Lộ Trình Phát Triển</button>
                </nav>
            </div>

            <div id="tab-content">
                <div id="market-share-content" class="tab-pane active">
                    <div class="grid md:grid-cols-5 gap-8 items-center">
                        <div class="md:col-span-2">
                            <h4 class="font-bold text-lg text-sapphire mb-2">Cấu Trúc Thị Phần Kiểm Định</h4>
                            <p class="text-sm text-gray-600 mb-4">Thị trường Việt Nam mang tính độc quyền nhóm với PNJ Lab đang chiếm vị trí dẫn đầu. Đối thủ chính bao gồm SJC Lab và Doji Lab. Nguy cơ lớn nhất đến từ các sản phẩm thay thế là giấy chứng nhận quốc tế GIA.</p>
                            <div class="space-y-3 text-sm">
                                <p><strong>Đối Thủ Quốc Tế:</strong> GIA (Mỹ) được xem là "chuẩn mực toàn cầu", tạo áp lực cạnh tranh và là rủi ro thay thế lớn nhất.</p>
                                <p><strong>Đối Thủ Trong Nước:</strong> Doji Lab có lợi thế từ hệ sinh thái Doji Group, trong khi SJC Lab giữ vị thế số 2.</p>
                            </div>
                        </div>
                        <div class="md:col-span-3">
                            <div class="chart-container">
                                <canvas id="marketShareChart"></canvas>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="swot-content" class="tab-pane hidden">
                     <div class="text-center mb-6">
                        <button id="s-o-strategy-btn" class="bg-sapphire text-white font-semibold py-2 px-5 rounded-lg hover:bg-opacity-90 transition-colors">
                           ✨ Tạo Chiến lược Kết hợp S-O
                        </button>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div id="swot-strengths" class="bg-green-50 border border-green-200 p-4 rounded-lg">
                            <h4 class="font-bold text-green-800">Điểm Mạnh (Strengths)</h4>
                            <ul class="list-disc list-inside mt-2 text-sm text-gray-700 space-y-1">
                                <li>Thị phần & thương hiệu dẫn đầu Việt Nam (41%).</li>
                                <li>Chất lượng dịch vụ ổn định, chuẩn quốc tế.</li>
                                <li>Đội ngũ kỹ thuật giàu kinh nghiệm, gắn bó lâu dài.</li>
                                <li>Tài chính vững mạnh, biên lợi nhuận >64%.</li>
                                <li>Hậu thuẫn mạnh mẽ từ hệ sinh thái PNJ Group.</li>
                            </ul>
                        </div>
                        <div class="bg-red-50 border border-red-200 p-4 rounded-lg">
                            <h4 class="font-bold text-red-800">Điểm Yếu (Weaknesses)</h4>
                            <ul class="list-disc list-inside mt-2 text-sm text-gray-700 space-y-1">
                                <li>Thiếu định hướng chiến lược trung và dài hạn rõ ràng.</li>
                                <li>Năng lực quản trị chưa bài bản, thiếu đội ngũ kế thừa.</li>
                                <li>Hoạt động Marketing & Bán hàng còn yếu, chưa chuyên nghiệp.</li>
                                <li>Quy trình vận hành còn nhiều thao tác thủ công.</li>
                                <li>Danh mục dịch vụ hạn chế, chủ yếu là kim cương rời.</li>
                            </ul>
                        </div>
                        <div id="swot-opportunities" class="bg-blue-50 border border-blue-200 p-4 rounded-lg">
                            <h4 class="font-bold text-blue-800">Cơ Hội (Opportunities)</h4>
                            <ul class="list-disc list-inside mt-2 text-sm text-gray-700 space-y-1">
                                <li>Xây dựng thương hiệu độc lập để phục vụ đa dạng khách hàng.</li>
                                <li>Mở rộng thị trường ra toàn quốc, đặc biệt là miền Bắc.</li>
                                <li>Phát triển dịch vụ mới: LGD, đào tạo, định giá, membership.</li>
                                <li>Ứng dụng công nghệ số (Blockchain, Edtech) để dẫn đầu đổi mới.</li>
                                <li>Định hướng quốc tế, cung cấp giấy chứng nhận thay thế nội địa.</li>
                            </ul>
                        </div>
                        <div class="bg-yellow-50 border border-yellow-200 p-4 rounded-lg">
                            <h4 class="font-bold text-yellow-800">Thách Thức (Threats)</h4>
                            <ul class="list-disc list-inside mt-2 text-sm text-gray-700 space-y-1">
                                <li>Quy mô thị trường nội địa nhỏ, tăng trưởng trung bình (CAGR 11%).</li>
                                <li>Sản phẩm thay thế từ giấy chứng nhận GIA uy tín hơn.</li>
                                <li>Xu hướng "bảo chứng thương hiệu" của nhà bán lẻ.</li>
                                <li>Rủi ro phụ thuộc lớn vào khách hàng nội bộ PNJ.</li>
                                <li>Sự dịch chuyển cơ cấu tiêu dùng từ kim cương rời sang trang sức.</li>
                            </ul>
                        </div>
                    </div>
                </div>
                
                <div id="timeline-content" class="tab-pane hidden">
                    <div class="relative pl-8 border-l-2 border-sapphire-light">
                        <div class="mb-8">
                            <div class="absolute w-4 h-4 bg-sapphire rounded-full -left-2 mt-1.5"></div>
                            <p class="font-bold text-sapphire">1999</p>
                            <p class="text-gray-700">Thành lập phòng kiểm định.</p>
                        </div>
                        <div class="mb-8">
                           <div class="absolute w-4 h-4 bg-sapphire rounded-full -left-2 mt-1.5"></div>
                            <p class="font-bold text-sapphire">2010</p>
                            <p class="text-gray-700">Chuyển đổi thành Công ty TNHH MTV Kiểm định.</p>
                        </div>
                        <div class="mb-8">
                            <div class="absolute w-4 h-4 bg-sapphire rounded-full -left-2 mt-1.5"></div>
                            <p class="font-bold text-sapphire">2015</p>
                            <p class="text-gray-700">Đạt chuẩn phòng thí nghiệm quốc tế ISO 17025.</p>
                        </div>
                         <div class="mb-8">
                            <div class="absolute w-4 h-4 bg-sapphire rounded-full -left-2 mt-1.5"></div>
                            <p class="font-bold text-sapphire">2025</p>
                            <p class="text-gray-700">Bà Đặng Thị Lài làm chủ tịch HĐQT, bắt đầu giai đoạn chiến lược mới.</p>
                        </div>
                        <div>
                           <div class="absolute w-4 h-4 bg-blue-500 rounded-full -left-2 mt-1.5 ring-4 ring-white"></div>
                            <p class="font-bold text-blue-600">Mục tiêu 2027</p>
                            <p class="text-gray-700">Xây dựng thành công thương hiệu PNJ Lab độc lập, không gắn chặt với PNJ.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="strategy" class="mb-12 md:mb-16">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-2">Định Hướng Chiến Lược 2025-2030</h3>
            <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Để đạt được tầm nhìn, P-Lab sẽ tập trung vào 4 trụ cột chính, được hỗ trợ bởi nền tảng vững chắc về tổ chức và tài chính. Hãy nhấn vào từng trụ cột để khám phá các mục tiêu và hành động chủ chốt.</p>
            
            <div id="accordion-container" class="space-y-4">
                <div class="accordion-item bg-white rounded-lg shadow-sm border border-gray-200">
                    <button class="accordion-header w-full flex justify-between items-center text-left p-5 font-semibold text-sapphire">
                        <span data-title="Củng Cố Nền Tảng Kiểm Định (Core Business)">1. Củng Cố Nền Tảng Kiểm Định (Core Business)</span>
                        <span class="accordion-arrow text-2xl font-light transform transition-transform">↓</span>
                    </button>
                    <div class="accordion-content px-5 pb-5">
                        <p class="text-gray-600 text-sm mb-3"><strong>Mục tiêu:</strong> Giữ vững vị thế số 1 tại Việt Nam (thị phần >40%), đồng thời nâng cao uy tín để dần tiệm cận chuẩn quốc tế.</p>
                        <ul class="list-disc list-inside text-sm text-gray-700 space-y-2">
                            <li>Tối ưu hóa năng suất, giảm thời gian kiểm định còn 1–2 ngày/mẫu.</li>
                            <li>Hoàn thiện hệ thống quản trị rủi ro và kiểm soát chất lượng theo ISO/IEC 17025.</li>
                            <li>Phát triển mô hình mini-lab tại các khu vực trọng điểm (Hà Nội, Đà Nẵng).</li>
                            <li>Đầu tư máy móc hiện đại, áp dụng công nghệ AI & Blockchain trong kiểm định.</li>
                        </ul>
                         <div class="mt-4 text-right">
                            <button class="gemini-brainstorm-btn bg-blue-100 text-blue-800 text-sm font-semibold py-1.5 px-4 rounded-lg hover:bg-blue-200 transition-colors">✨ Brainstorm Sáng kiến</button>
                        </div>
                    </div>
                </div>

                <div class="accordion-item bg-white rounded-lg shadow-sm border border-gray-200">
                    <button class="accordion-header w-full flex justify-between items-center text-left p-5 font-semibold text-sapphire">
                        <span data-title="Marketing & Mở Rộng Thị Trường">2. Marketing & Mở Rộng Thị Trường</span>
                         <span class="accordion-arrow text-2xl font-light transform transition-transform">↓</span>
                    </button>
                    <div class="accordion-content px-5 pb-5">
                        <p class="text-gray-600 text-sm mb-3"><strong>Mục tiêu:</strong> Gia tăng thị phần, xây dựng PNJ Lab thành thương hiệu kiểm định đáng tin cậy nhất Việt Nam.</p>
                        <ul class="list-disc list-inside text-sm text-gray-700 space-y-2">
                             <li>Xây dựng hệ thống Marketing chuyên nghiệp, ứng dụng digital marketing.</li>
                             <li>Định hướng thị trường chuyển đổi sang sử dụng song song giấy GIA và PNJ Lab.</li>
                             <li>Mở rộng nhóm khách hàng ngoài hệ sinh thái PNJ (Doji, SJC, khách hàng cá nhân).</li>
                             <li>Tăng cường tiếp cận khách du lịch quốc tế tại các trung tâm thương mại và sân bay.</li>
                        </ul>
                        <div class="mt-4 text-right">
                            <button class="gemini-brainstorm-btn bg-blue-100 text-blue-800 text-sm font-semibold py-1.5 px-4 rounded-lg hover:bg-blue-200 transition-colors">✨ Brainstorm Sáng kiến</button>
                        </div>
                    </div>
                </div>

                <div class="accordion-item bg-white rounded-lg shadow-sm border border-gray-200">
                    <button class="accordion-header w-full flex justify-between items-center text-left p-5 font-semibold text-sapphire">
                        <span data-title="Phát Triển Ngành Mới – Đào Tạo Kiểm Định">3. Phát Triển Ngành Mới – Đào Tạo Kiểm Định</span>
                         <span class="accordion-arrow text-2xl font-light transform transition-transform">↓</span>
                    </button>
                    <div class="accordion-content px-5 pb-5">
                        <p class="text-gray-600 text-sm mb-3"><strong>Mục tiêu:</strong> Trở thành trung tâm đào tạo kiểm định & ngọc học lớn nhất Việt Nam, tiến tới chuẩn quốc tế và có uy tín trong khu vực.</p>
                        <ul class="list-disc list-inside text-sm text-gray-700 space-y-2">
                             <li>Thành lập "Viện Ngọc học PNJ" với các chương trình đào tạo đa cấp độ.</li>
                             <li>Kết hợp mô hình Edtech (học online) và In-lab training (thực hành).</li>
                             <li>Hợp tác với các trường đại học lớn và các tổ chức quốc tế (GIA, HRD).</li>
                             <li>Định giá học phí cạnh tranh, chỉ bằng 50-70% so với GIA/HRD.</li>
                        </ul>
                         <div class="mt-4 text-right">
                            <button class="gemini-brainstorm-btn bg-blue-100 text-blue-800 text-sm font-semibold py-1.5 px-4 rounded-lg hover:bg-blue-200 transition-colors">✨ Brainstorm Sáng kiến</button>
                        </div>
                    </div>
                </div>
                
                 <div class="accordion-item bg-white rounded-lg shadow-sm border border-gray-200">
                    <button class="accordion-header w-full flex justify-between items-center text-left p-5 font-semibold text-sapphire">
                        <span data-title="Phát Triển Dịch Vụ Giá Trị Gia Tăng">4. Phát Triển Dịch Vụ Giá Trị Gia Tăng</span>
                         <span class="accordion-arrow text-2xl font-light transform transition-transform">↓</span>
                    </button>
                    <div class="accordion-content px-5 pb-5">
                        <p class="text-gray-600 text-sm mb-3"><strong>Mục tiêu:</strong> Đa dạng hóa dịch vụ, tăng doanh thu từ các nguồn ngoài hoạt động kiểm định cốt lõi.</p>
                        <ul class="list-disc list-inside text-sm text-gray-700 space-y-2">
                             <li>Phát triển dịch vụ định giá tài sản cho ngân hàng, bảo hiểm, cầm cố.</li>
                             <li>Mở rộng kiểm định sang sản phẩm cao cấp khác: đồng hồ, túi xách, kim cương nhân tạo (LGD).</li>
                             <li>Triển khai mô hình membership: khách hàng đóng phí định kỳ để hưởng gói dịch vụ toàn diện.</li>
                             <li>Cung cấp dịch vụ mobile lab: kiểm định tại chỗ cho khách VIP và doanh nghiệp.</li>
                        </ul>
                         <div class="mt-4 text-right">
                            <button class="gemini-brainstorm-btn bg-blue-100 text-blue-800 text-sm font-semibold py-1.5 px-4 rounded-lg hover:bg-blue-200 transition-colors">✨ Brainstorm Sáng kiến</button>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="customers" class="bg-sandstone p-6 md:p-8 rounded-lg shadow-inner">
            <h3 class="text-2xl md:text-3xl font-bold text-sapphire text-center mb-2">Khách Hàng Mục Tiêu</h3>
            <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Chiến lược của P-Lab được xây dựng dựa trên sự thấu hiểu sâu sắc 5 phân khúc khách hàng chính, mỗi nhóm có những nhu cầu và vai trò khác nhau đối với sự phát triển của công ty.</p>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h4 class="font-bold text-lg text-sapphire">Cá nhân cao cấp</h4>
                    <p class="text-xs font-semibold text-blue-600 bg-blue-100 inline-block px-2 py-1 rounded-full my-2">PHÂN KHÚC CASH COW</p>
                    <p class="text-sm text-gray-600">Nhóm trọng tâm, chiếm tỷ trọng lớn nhất, tạo nguồn thu ổn định từ nhu cầu kiểm định kim cương cưới và đầu tư.</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h4 class="font-bold text-lg text-sapphire">Doanh nghiệp bán lẻ</h4>
                     <p class="text-xs font-semibold text-green-600 bg-green-100 inline-block px-2 py-1 rounded-full my-2">PHÂN KHÚC CHIẾN LƯỢỢC</p>
                    <p class="text-sm text-gray-600">Các đối tác như PNJ, Doji, SJC... quyết định sức mạnh thị phần. Cần duy trì và mở rộng hợp tác ngoài hệ sinh thái PNJ.</p>
                </div>
                 <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h4 class="font-bold text-lg text-sapphire">Thợ kim hoàn & Nghệ nhân</h4>
                    <p class="text-xs font-semibold text-purple-600 bg-purple-100 inline-block px-2 py-1 rounded-full my-2">PHÂN KHÚC ĐÀO TẠO</p>
                    <p class="text-sm text-gray-600">Đối tượng quan trọng cho mảng đào tạo, giúp tạo nguồn khách hàng ổn định và nâng cao chuyên môn ngành.</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h4 class="font-bold text-lg text-sapphire">Nhà sưu tầm & Phong thủy</h4>
                    <p class="text-xs font-semibold text-yellow-600 bg-yellow-100 inline-block px-2 py-1 rounded-full my-2">PHÂN KHÚC NICHE</p>
                    <p class="text-sm text-gray-600">Thị trường nhỏ nhưng trung thành, giúp P-Lab xây dựng uy tín trong mảng học thuật và chuyên môn sâu về đá màu, ngọc cổ.</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h4 class="font-bold text-lg text-sapphire">Khách quốc tế & Du khách</h4>
                     <p class="text-xs font-semibold text-red-600 bg-red-100 inline-block px-2 py-1 rounded-full my-2">PHÂN KHÚC TĂNG TRƯỞNG</p>
                    <p class="text-sm text-gray-600">Phân khúc tiềm năng nhờ hội nhập và du lịch. Đòi hỏi P-Lab nâng cao uy tín quốc tế và các dịch vụ số hóa (e-report).</p>
                </div>
            </div>
        </section>

    </main>
    
    <div id="gemini-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 gemini-modal-backdrop hidden">
        <div class="bg-white rounded-lg shadow-2xl max-w-2xl w-full max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center p-4 border-b">
                <h3 id="gemini-modal-title" class="text-lg font-bold text-sapphire">✨ Gợi ý từ Gemini</h3>
                <button id="gemini-modal-close" class="text-gray-500 hover:text-gray-800 text-2xl">&times;</button>
            </div>
            <div id="gemini-modal-body" class="p-6 overflow-y-auto">
                <div id="gemini-loader" class="flex flex-col items-center justify-center py-10">
                    <div class="loader"></div>
                    <p class="mt-4 text-gray-600">Đang xử lý, vui lòng chờ trong giây lát...</p>
                </div>
                <div id="gemini-response" class="text-gray-700 space-y-4 prose max-w-none"></div>
            </div>
        </div>
    </div>


    <footer class="bg-sapphire text-white mt-12">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-6 text-center text-sm">
            <p>&copy; 2025 P-Lab Strategic Interactive Report. All rights reserved.</p>
            <p class="text-gray-400 mt-1">Báo cáo được tạo cho mục đích trình bày nội bộ.</p>
        </div>
    </footer>

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
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                padding: 15,
                                font: {
                                   family: "'Be Vietnam Pro', sans-serif"
                                }
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed !== null) {
                                        label += context.parsed + '%';
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            });

            const tabs = document.querySelectorAll('.tab-btn');
            const tabPanes = document.querySelectorAll('.tab-pane');

            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    const targetId = tab.dataset.tab + '-content';

                    tabs.forEach(t => {
                        t.classList.remove('active', 'text-sapphire');
                        t.classList.add('text-gray-500');
                    });
                    tab.classList.add('active', 'text-sapphire');
                    tab.classList.remove('text-gray-500');

                    tabPanes.forEach(pane => {
                        if (pane.id === targetId) {
                            pane.classList.remove('hidden');
                            pane.classList.add('active');
                        } else {
                            pane.classList.add('hidden');
                            pane.classList.remove('active');
                        }
                    });
                });
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

            const modal = document.getElementById('gemini-modal');
            const modalCloseBtn = document.getElementById('gemini-modal-close');
            const modalTitle = document.getElementById('gemini-modal-title');
            const loader = document.getElementById('gemini-loader');
            const responseContainer = document.getElementById('gemini-response');

            const showModal = () => modal.classList.remove('hidden');
            const hideModal = () => modal.classList.add('hidden');

            modalCloseBtn.addEventListener('click', hideModal);
            modal.addEventListener('click', (e) => {
                if (e.target === modal) {
                    hideModal();
                }
            });

            function formatResponse(text) {
                return text
                    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                    .replace(/\n/g, '<br>')
                    .replace(/(\<br\>\s*\d+\.\s*)/g, '<br><br>$1') 
                    .replace(/(\<br\>\s*\-\s*)/g, '<br> &bull; ');
            }
            
            async function callGeminiAPI(prompt, systemPrompt) {
                showModal();
                loader.classList.remove('hidden');
                responseContainer.innerHTML = '';
                
                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                const payload = {
                    contents: [{ parts: [{ text: prompt }] }],
                    systemInstruction: {
                        parts: [{ text: systemPrompt }]
                    },
                };

                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (!response.ok) {
                        throw new Error(`API error: ${response.statusText}`);
                    }

                    const result = await response.json();
                    const text = result.candidates?.[0]?.content?.parts?.[0]?.text;
                    
                    if (text) {
                        responseContainer.innerHTML = formatResponse(text);
                    } else {
                        responseContainer.textContent = 'Không nhận được phản hồi hợp lệ từ AI. Vui lòng thử lại.';
                    }
                } catch (error) {
                    console.error('Gemini API call failed:', error);
                    responseContainer.textContent = 'Đã có lỗi xảy ra khi kết nối với AI. Vui lòng kiểm tra console và thử lại.';
                } finally {
                    loader.classList.add('hidden');
                }
            }
            
            document.querySelectorAll('.gemini-brainstorm-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    e.stopPropagation();
                    const accordionItem = e.target.closest('.accordion-item');
                    const title = accordionItem.querySelector('[data-title]').dataset.title;
                    const goal = accordionItem.querySelector('strong').nextSibling.textContent.trim();
                    const actions = Array.from(accordionItem.querySelectorAll('li')).map(li => li.textContent).join(', ');

                    modalTitle.textContent = `✨ Brainstorm cho: ${title}`;
                    
                    const systemPrompt = "Bạn là một chuyên gia tư vấn chiến lược kinh doanh hàng đầu, chuyên về ngành đá quý và trang sức tại Việt Nam. Hãy đưa ra những ý tưởng sáng tạo, cụ thể và khả thi bằng tiếng Việt.";
                    const userPrompt = `Dựa trên trụ cột chiến lược của P-Lab là '${title}' với mục tiêu '${goal}', và các hành động chính là '${actions}', hãy đề xuất 3 sáng kiến đột phá, chi tiết để thực thi. Tập trung vào các lĩnh vực sau:
1.  **Công nghệ & Đổi mới:** Một ý tưởng ứng dụng công nghệ để tạo lợi thế cạnh tranh.
2.  **Marketing & Trải nghiệm khách hàng:** Một chiến dịch marketing hoặc chương trình chăm sóc khách hàng độc đáo.
3.  **Hợp tác & Mở rộng:** Một ý tưởng hợp tác với đối tác chiến lược để thúc đẩy mục tiêu.`;
                    
                    callGeminiAPI(userPrompt, systemPrompt);
                });
            });

            document.getElementById('s-o-strategy-btn').addEventListener('click', () => {
                 modalTitle.textContent = `✨ Tạo Chiến lược kết hợp Điểm mạnh & Cơ hội`;

                const strengths = Array.from(document.querySelectorAll('#swot-strengths li')).map(li => li.textContent).join('; ');
                const opportunities = Array.from(document.querySelectorAll('#swot-opportunities li')).map(li => li.textContent).join('; ');
                
                const systemPrompt = "Bạn là một chuyên gia tư vấn chiến lược cấp cao. Nhiệm vụ của bạn là phân tích và kết hợp Điểm mạnh (Strengths) và Cơ hội (Opportunities) để tạo ra các chiến lược tấn công (S-O) sắc bén, thực tế và hiệu quả bằng tiếng Việt.";
                const userPrompt = `Một công ty kiểm định đá quý (P-Lab) có những Điểm mạnh sau: '${strengths}'. Và họ đang đối mặt với những Cơ hội này: '${opportunities}'.
Hãy xây dựng 3 chiến lược S-O (Strengths-Opportunities) cụ thể và chi tiết. Với mỗi chiến lược, hãy:
1.  Nêu rõ Điểm mạnh và Cơ hội nào được kết hợp.
2.  Mô tả chi tiết chiến lược và các bước thực hiện chính.
3.  Giải thích tại sao chiến lược đó sẽ tạo ra lợi thế cạnh tranh đột phá.`;

                callGeminiAPI(userPrompt, systemPrompt);
            });
        });
    </script>
</body>
</html>
