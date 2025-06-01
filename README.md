<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>بنك قناة السويس | الخدمات المصرفية</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            scroll-behavior: smooth;
        }
        .chart-container {
            position: relative;
            height: 300px;
            max-height: 40vh;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }
        .tab-button.active {
            border-color: #0e7490;
            background-color: #0e7490;
            color: white;
        }
        .section-hidden, .page-hidden {
            display: none !important;
        }
        .modal-screen, #product-choice-screen {
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
        }
        #app-content-wrapper {
            transition: opacity 0.5s ease-in-out;
        }
        .choice-button {
            transition: transform 0.2s ease-out, box-shadow 0.2s ease-out;
        }
        .choice-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }
        .bank-logo-text {
            font-weight: 700;
            color: #0891b2; /* Tailwind cyan-600 */
            font-size: 1.75rem; /* text-2xl */
            line-height: 2.25rem; /* leading-9 */
        }
        .bank-logo-subtext {
            font-size: 0.75rem; /* text-xs */
            color: #4b5563; /* Tailwind gray-600 */
            display: block;
            line-height: 1;
            margin-top: -2px;
        }
        .back-button {
            position: absolute;
            top: 20px;
            left: 20px;
            background-color: #0e7490;
            color: white;
            padding: 8px 12px;
            border-radius: 9999px; /* rounded-full */
            font-size: 1.5rem; /* text-2xl for arrow size */
            line-height: 1;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            z-index: 60; /* Ensure it's above content but below modals */
        }
         .back-button:hover {
            background-color: #087287;
        }

    </style>
</head>
<body class="bg-slate-50 text-gray-800">

    <div id="login-modal" class="modal-screen fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-[100]">
        <div class="bg-white p-8 rounded-lg shadow-xl w-full max-w-sm mx-4 sm:max-w-md">
            <div class="flex justify-center mb-6">
                <div class="bank-logo-text">بنك قناة السويس</div>
            </div>
            <h2 class="text-2xl font-bold text-center text-cyan-700 mb-6">تسجيل الدخول</h2>
            <form id="login-form">
                <div class="mb-4">
                    <label for="username" class="block text-sm font-medium text-gray-700 mb-1">اسم المستخدم</label>
                    <input type="text" id="username" name="username" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 placeholder-gray-400" placeholder="ادخل اسم المستخدم" required>
                </div>
                <div class="mb-6">
                    <label for="password" class="block text-sm font-medium text-gray-700 mb-1">كلمة المرور</label>
                    <input type="password" id="password" name="password" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 placeholder-gray-400" placeholder="ادخل كلمة المرور" required>
                </div>
                <div id="login-error" class="text-red-600 text-sm mb-4 text-center font-semibold" style="display: none;"></div>
                <button type="submit" class="w-full bg-cyan-700 text-white py-2.5 px-4 rounded-md hover:bg-cyan-800 focus:outline-none focus:ring-2 focus:ring-cyan-600 focus:ring-opacity-75 transition duration-200 ease-in-out font-semibold">
                    دخـــول
                </button>
            </form>
        </div>
    </div>

    <div id="product-choice-screen" class="modal-screen fixed inset-0 bg-slate-100 flex items-center justify-center z-[90]" style="display: none; opacity: 0; visibility: hidden;">
        <div class="bg-white p-8 sm:p-12 rounded-xl shadow-2xl w-full max-w-lg mx-4 text-center">
            <div class="flex justify-center mb-8">
                <div class="bank-logo-text">بنك قناة السويس</div>
            </div>
            <h2 class="text-3xl font-bold text-cyan-800 mb-8">اختر الخدمة المطلوبة</h2>
            <p class="text-gray-600 mb-10 text-lg">مرحباً بك <span id="welcome-username" class="font-semibold text-cyan-700"></span>! يرجى اختيار نوع المنتج الذي ترغب في استكشافه.</p>
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-6">
                <button data-choice="certificates" class="choice-button bg-cyan-600 text-white py-8 px-4 rounded-lg shadow-md hover:bg-cyan-700 focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:ring-opacity-75">
                    <span class="text-5xl block mb-3">📄</span>
                    <span class="text-xl font-semibold">الشهادات</span>
                </button>
                <button data-choice="loans" class="choice-button bg-orange-500 text-white py-8 px-4 rounded-lg shadow-md hover:bg-orange-600 focus:outline-none focus:ring-2 focus:ring-orange-400 focus:ring-opacity-75">
                    <span class="text-5xl block mb-3">💰</span>
                    <span class="text-xl font-semibold">القروض</span>
                </button>
                <button data-choice="deposits" class="choice-button bg-teal-500 text-white py-8 px-4 rounded-lg shadow-md hover:bg-teal-600 focus:outline-none focus:ring-2 focus:ring-teal-400 focus:ring-opacity-75">
                     <span class="text-5xl block mb-3">🏦</span>
                    <span class="text-xl font-semibold">الودائع</span>
                </button>
            </div>
             <button id="logout-button-choice" class="mt-10 text-sm text-gray-500 hover:text-cyan-700 underline">تسجيل الخروج</button>
        </div>
    </div>

    <div id="app-content-wrapper" class="page-hidden" style="opacity: 0;">
        <button id="back-to-choice-button" title="العودة لاختيار الخدمة" class="back-button page-hidden">
            <span>&#x279C;</span> </button>
        <header class="bg-white shadow-md sticky top-0 z-50">
            <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex items-center justify-between h-16">
                    <div class="flex items-center">
                        <a href="#" id="header-logo-link" class="bank-logo-text">بنك قناة السويس</a>
                    </div>
                    <div class="hidden md:block">
                        <div id="main-nav-links" class="ml-10 flex items-baseline space-x-4 space-x-reverse">
                            <a href="#hero" class="nav-link text-gray-600 hover:text-cyan-700 px-3 py-2 rounded-md text-sm font-medium">الرئيسية</a>
                            <a href="#products" class="nav-link text-gray-600 hover:text-cyan-700 px-3 py-2 rounded-md text-sm font-medium">المنتجات</a>
                            <a href="#calculator" class="nav-link text-gray-600 hover:text-cyan-700 px-3 py-2 rounded-md text-sm font-medium">الحاسبة</a>
                            <a href="#contact" class="nav-link text-gray-600 hover:text-cyan-700 px-3 py-2 rounded-md text-sm font-medium">تواصل معنا</a>
                            <button id="logout-button-header" class="text-gray-600 hover:text-red-600 px-3 py-2 rounded-md text-sm font-medium">تسجيل الخروج</button>
                        </div>
                    </div>
                    <div class="md:hidden">
                        <button id="mobile-menu-button" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-cyan-700 focus:outline-none">
                            <span class="sr-only">فتح القائمة الرئيسية</span>
                            <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
                            </svg>
                        </button>
                    </div>
                </div>
            </nav>
            <div id="mobile-menu" class="md:hidden hidden">
                <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                    <a href="#hero" class="nav-link text-gray-600 hover:bg-cyan-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">الرئيسية</a>
                    <a href="#products" class="nav-link text-gray-600 hover:bg-cyan-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">المنتجات</a>
                    <a href="#calculator" class="nav-link text-gray-600 hover:bg-cyan-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">الحاسبة</a>
                    <a href="#contact" class="nav-link text-gray-600 hover:bg-cyan-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">تواصل معنا</a>
                    <button id="logout-button-mobile" class="w-full text-left text-gray-600 hover:bg-red-600 hover:text-white block px-3 py-2 rounded-md text-base font-medium">تسجيل الخروج</button>
                </div>
            </div>
        </header>

        <main>
            <section id="hero" class="page-section bg-cyan-800 text-white pt-20 pb-24">
                <div class="container mx-auto px-4 sm:px-6 lg:px-8 text-center">
                    <h1 id="hero-title" class="text-4xl md:text-5xl font-bold leading-tight mb-4">استثمر مستقبلك بثقة</h1>
                    <p id="hero-subtitle" class="text-lg md:text-xl text-cyan-200 max-w-3xl mx-auto mb-8">نظرة شاملة على أبرز منتجاتنا المصرفية.</p>
                    <a href="#products" class="bg-white text-cyan-800 font-bold py-3 px-8 rounded-full hover:bg-cyan-100 transition duration-300">اكتشف المنتجات</a>
                </div>
            </section>

            <section id="products" class="page-section py-16 sm:py-20">
                <div class="container mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center mb-12">
                        <h2 id="products-title" class="text-3xl md:text-4xl font-bold text-gray-900">منتجاتنا المصرفية</h2>
                        <p id="products-subtitle" class="mt-4 text-lg text-gray-600 max-w-2xl mx-auto">(البيانات المعروضة هي أمثلة وقد تختلف عن الأسعار الفعلية بالبنك)</p>
                    </div>
                    <div id="products-grid-container">
                        <div id="certificates-grid" class="grid md:grid-cols-2 lg:grid-cols-3 gap-8 product-grid-content page-hidden"></div>
                        <div id="deposits-grid" class="grid md:grid-cols-2 lg:grid-cols-3 gap-8 product-grid-content page-hidden"></div>
                        <div id="loans-grid" class="grid md:grid-cols-2 lg:grid-cols-3 gap-8 product-grid-content page-hidden"></div>
                    </div>
                </div>
            </section>

            <section id="calculator" class="page-section py-16 sm:py-20 bg-gray-100">
                <div class="container mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center mb-12">
                        <h2 class="text-3xl md:text-4xl font-bold text-gray-900">الحاسبة التفاعلية</h2>
                        <p id="calculator-subtitle" class="mt-4 text-lg text-gray-600 max-w-2xl mx-auto">(الأرقام المستخدمة في الحاسبة هي أمثلة توضيحية)</p>
                    </div>
                    <div class="max-w-4xl mx-auto grid md:grid-cols-2 gap-8 items-start">
                        <div id="certificate-calculator-wrapper" class="bg-white p-6 rounded-lg shadow-lg page-hidden">
                            <h3 class="text-xl font-bold mb-4 text-cyan-800">حاسبة أرباح الشهادات</h3>
                            <div class="space-y-4">
                                <div>
                                    <label for="cert-amount" class="block text-sm font-medium text-gray-700">مبلغ الاستثمار (ج.م)</label>
                                    <input type="number" id="cert-amount" value="10000" min="1000" step="1000" class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-cyan-500 focus:border-cyan-500">
                                </div>
                                <div>
                                    <label for="cert-rate" class="block text-sm font-medium text-gray-700">سعر العائد السنوي (%)</label>
                                    <input type="number" id="cert-rate" value="20" min="1" max="30" step="0.1" class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-cyan-500 focus:border-cyan-500">
                                </div>
                                 <div>
                                    <label for="cert-years" class="block text-sm font-medium text-gray-700">مدة الشهادة (سنوات)</label>
                                    <input type="number" id="cert-years" value="3" min="1" max="10" class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-cyan-500 focus:border-cyan-500">
                                </div>
                            </div>
                            <div class="mt-6">
                                <div class="chart-container">
                                    <canvas id="earningsChart"></canvas>
                                </div>
                            </div>
                        </div>
                        <div id="loan-calculator-wrapper" class="bg-white p-6 rounded-lg shadow-lg page-hidden">
                            <h3 class="text-xl font-bold mb-4 text-cyan-800">حاسبة أقساط القروض</h3>
                            <div class="space-y-4">
                                <div>
                                    <label for="loan-amount" class="block text-sm font-medium text-gray-700">مبلغ القرض (ج.م)</label>
                                    <input type="number" id="loan-amount" value="50000" min="5000" step="1000" class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-cyan-500 focus:border-cyan-500">
                                </div>
                                <div>
                                    <label for="loan-years" class="block text-sm font-medium text-gray-700">فترة السداد (سنوات)</label>
                                    <input type="number" id="loan-years" value="5" min="1" max="15" class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-cyan-500 focus:border-cyan-500">
                                </div>
                                 <div>
                                    <label for="loan-rate" class="block text-sm font-medium text-gray-700">سعر الفائدة السنوي (%)</label>
                                    <input type="number" id="loan-rate" value="22.5" min="5" max="35" step="0.1" class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-cyan-500 focus:border-cyan-500">
                                </div>
                            </div>
                             <div class="mt-6 text-center">
                                <p class="text-gray-600">القسط الشهري المقدر:</p>
                                <p id="monthly-payment" class="text-2xl font-bold text-cyan-700">0.00 جنيه</p>
                            </div>
                            <div class="mt-4">
                                 <div class="chart-container" style="height:250px;">
                                    <canvas id="loanChart"></canvas>
                                </div>
                            </div>
                        </div>
                         <div id="deposit-calculator-placeholder" class="bg-white p-6 rounded-lg shadow-lg page-hidden text-center text-gray-500">
                            <h3 class="text-xl font-bold mb-4 text-cyan-800">حاسبة الودائع</h3>
                            <p>سيتم إضافة حاسبة تفاعلية للودائع قريباً.</p>
                            <span class="text-6xl mt-4 block">⏱️</span>
                        </div>
                    </div>
                </div>
            </section>
            
            <section id="contact" class="page-section bg-cyan-800 text-white">
                <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-16">
                    <div class="text-center mb-10">
                        <h2 class="text-3xl md:text-4xl font-bold">جاهز لتبدأ رحلتك المالية؟</h2>
                        <p class="mt-4 text-lg text-cyan-200 max-w-2xl mx-auto">تواصل معنا اليوم أو قم بزيارة أقرب فرع. فريقنا مستعد لمساعدتك في اختيار المنتج الأنسب لك.</p>
                    </div>
                    <div class="max-w-4xl mx-auto grid md:grid-cols-3 gap-8 text-center">
                        <div class="bg-cyan-700/50 p-6 rounded-lg">
                            <h3 class="text-xl font-bold mb-2">تفضل بزيارتنا</h3>
                            <p class="text-cyan-200">ابحث عن أقرب فرع من شبكة فروعنا الواسعة في جميع أنحاء الجمهورية.</p>
                        </div>
                        <div class="bg-cyan-700/50 p-6 rounded-lg">
                            <h3 class="text-xl font-bold mb-2">اتصل بنا</h3>
                            <p class="text-cyan-200">تحدث مباشرة مع خدمة العملاء على الخط الساخن: <span class="font-bold tracking-wider">19093</span> (مثال)</p>
                        </div>
                        <div class="bg-cyan-700/50 p-6 rounded-lg">
                            <h3 class="text-xl font-bold mb-2">تابعنا أونلاين</h3>
                            <p class="text-cyan-200">اكتشف المزيد عبر موقعنا الإلكتروني وتطبيقاتنا المصرفية الرقمية.</p>
                        </div>
                    </div>
                </div>
            </section>
        </main>
        
        <footer class="bg-gray-900 text-white">
            <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-6 text-center">
                <p>&copy; <span id="current-year"></span> بنك قناة السويس. جميع الحقوق محفوظة.</p>
                <p class="text-sm text-gray-400 mt-2">هذا عرض توضيحي تفاعلي. البيانات المعروضة هي أمثلة وقد تختلف عن الأسعار والشروط الفعلية. للحصول على معلومات دقيقة ومحدثة، يرجى الرجوع إلى الموقع الرسمي للبنك أو التواصل مع خدمة العملاء.</p>
            </div>
        </footer>
    </div>

<script>
document.addEventListener('DOMContentLoaded', function () {
    const loginModal = document.getElementById('login-modal');
    const loginForm = document.getElementById('login-form');
    const loginError = document.getElementById('login-error');
    const productChoiceScreen = document.getElementById('product-choice-screen');
    const appContentWrapper = document.getElementById('app-content-wrapper');
    const usernameInput = document.getElementById('username');
    const passwordInput = document.getElementById('password');
    const welcomeUsernameSpan = document.getElementById('welcome-username');
    const backToChoiceButton = document.getElementById('back-to-choice-button');
    const headerLogoLink = document.getElementById('header-logo-link');

    const allPageSections = document.querySelectorAll('.page-section');
    const allProductGridContents = document.querySelectorAll('.product-grid-content');
    const certificateCalculatorWrapper = document.getElementById('certificate-calculator-wrapper');
    const loanCalculatorWrapper = document.getElementById('loan-calculator-wrapper');
    const depositCalculatorPlaceholder = document.getElementById('deposit-calculator-placeholder');
    
    const heroTitle = document.getElementById('hero-title');
    const heroSubtitle = document.getElementById('hero-subtitle');
    const productsTitle = document.getElementById('products-title');
    const productsSubtitle = document.getElementById('products-subtitle');
    const calculatorSubtitle = document.getElementById('calculator-subtitle');

    const serviceSpecificText = {
        certificates: {
            heroTitle: "شهادات بنك قناة السويس",
            heroSubtitle: "استثمر بأمان وحقق عوائد تنافسية مع باقة شهاداتنا المتنوعة.",
            productsTitle: "أنواع الشهادات الادخارية",
            productsSubtitle: "اختر الشهادة التي تناسب أهدافك الاستثمارية.",
            calculatorSubtitle: "احسب أرباح شهادتك الادخارية المتوقعة."
        },
        loans: {
            heroTitle: "قروض بنك قناة السويس",
            heroSubtitle: "حقق طموحاتك مع حلولنا التمويلية المرنة والميسرة.",
            productsTitle: "برامج القروض المتاحة",
            productsSubtitle: "اكتشف برنامج القرض الذي يلبي احتياجاتك المالية.",
            calculatorSubtitle: "احسب القسط الشهري لقرضك بسهولة."
        },
        deposits: {
            heroTitle: "ودائع بنك قناة السويس",
            heroSubtitle: "ادخر أموالك بمرونة وأمان مع حسابات الودائع المتنوعة.",
            productsTitle: "أنواع الودائع وحسابات التوفير",
            productsSubtitle: "اختر الوعاء الادخاري الأنسب لك.",
            calculatorSubtitle: "نعمل على توفير حاسبة تفاعلية للودائع قريباً."
        }
    };

    function resetAppContent() {
        allPageSections.forEach(s => s.classList.add('page-hidden'));
        allProductGridContents.forEach(pg => pg.classList.add('page-hidden'));
        certificateCalculatorWrapper.classList.add('page-hidden');
        loanCalculatorWrapper.classList.add('page-hidden');
        depositCalculatorPlaceholder.classList.add('page-hidden');
        backToChoiceButton.classList.add('page-hidden');
    }

    function showProductChoiceScreen() {
        resetAppContent();
        appContentWrapper.style.opacity = '0';
        appContentWrapper.classList.add('page-hidden');
        setTimeout(() => {
            appContentWrapper.style.display = 'none';
            productChoiceScreen.style.display = 'flex';
            setTimeout(() => {
                productChoiceScreen.style.opacity = '1';
                productChoiceScreen.style.visibility = 'visible';
            }, 50);
        }, 300);
    }
    
    if(headerLogoLink) {
        headerLogoLink.addEventListener('click', function(e){
            e.preventDefault();
            showProductChoiceScreen();
        });
    }


    loginForm.addEventListener('submit', function(event) {
        event.preventDefault();
        const username = usernameInput.value.trim();
        const password = passwordInput.value.trim();

        if (username === "مصطفي" && password === "كاستلو") {
            loginModal.style.opacity = '0';
            loginModal.style.visibility = 'hidden';
            setTimeout(() => {
                loginModal.style.display = 'none';
                if (welcomeUsernameSpan) welcomeUsernameSpan.textContent = username;
                showProductChoiceScreen();
            }, 300); 
        } else {
            loginError.textContent = 'اسم المستخدم أو كلمة المرور غير صحيحة. الرجاء المحاولة مرة أخرى.';
            loginError.style.display = 'block';
            passwordInput.value = ''; 
            usernameInput.focus();
        }
    });
    
    const logoutButtons = document.querySelectorAll('#logout-button-choice, #logout-button-header, #logout-button-mobile');
    logoutButtons.forEach(btn => {
        btn.addEventListener('click', function() {
            usernameInput.value = '';
            passwordInput.value = '';
            loginError.style.display = 'none';
            
            appContentWrapper.style.opacity = '0';
            productChoiceScreen.style.opacity = '0';
            productChoiceScreen.style.visibility = 'hidden';

            setTimeout(() => {
                appContentWrapper.style.display = 'none';
                appContentWrapper.classList.add('page-hidden');
                productChoiceScreen.style.display = 'none';
                
                loginModal.style.display = 'flex';
                setTimeout(() => {
                    loginModal.style.opacity = '1';
                    loginModal.style.visibility = 'visible';
                    usernameInput.focus();
                }, 50);
            }, 300);
        });
    });


    const choiceButtons = document.querySelectorAll('.choice-button');
    choiceButtons.forEach(button => {
        button.addEventListener('click', function() {
            const choice = this.dataset.choice;
            productChoiceScreen.style.opacity = '0';
            productChoiceScreen.style.visibility = 'hidden';
            setTimeout(() => {
                productChoiceScreen.style.display = 'none';
                appContentWrapper.style.display = 'block';
                appContentWrapper.classList.remove('page-hidden');
                setTimeout(() => {
                    appContentWrapper.style.opacity = '1';
                    configureAppForService(choice);
                }, 50);
            }, 300);
        });
    });

    if(backToChoiceButton) {
        backToChoiceButton.addEventListener('click', showProductChoiceScreen);
    }


    function configureAppForService(serviceType) {
        resetAppContent(); // Hide everything first

        const texts = serviceSpecificText[serviceType];
        if (texts) {
            if(heroTitle) heroTitle.textContent = texts.heroTitle;
            if(heroSubtitle) heroSubtitle.textContent = texts.heroSubtitle;
            if(productsTitle) productsTitle.textContent = texts.productsTitle;
            if(productsSubtitle) productsSubtitle.textContent = texts.productsSubtitle;
            if(calculatorSubtitle) calculatorSubtitle.textContent = texts.calculatorSubtitle;
        }

        document.querySelectorAll('#hero, #products, #calculator, #contact').forEach(s => s.classList.remove('page-hidden'));
        backToChoiceButton.classList.remove('page-hidden');


        if (serviceType === "certificates") {
            document.getElementById('certificates-grid').classList.remove('page-hidden');
            if(certificateCalculatorWrapper) certificateCalculatorWrapper.classList.remove('page-hidden');
        } else if (serviceType === "loans") {
            document.getElementById('loans-grid').classList.remove('page-hidden');
            if(loanCalculatorWrapper) loanCalculatorWrapper.classList.remove('page-hidden');
        } else if (serviceType === "deposits") {
            document.getElementById('deposits-grid').classList.remove('page-hidden');
            if(depositCalculatorPlaceholder) depositCalculatorPlaceholder.classList.remove('page-hidden');
        }
        
        // Scroll to hero section of the "page"
        const heroSectionEl = document.getElementById('hero');
        if (heroSectionEl) {
            setTimeout(() => { // Delay scroll slightly for content to render
                 window.scrollTo({ top: heroSectionEl.offsetTop - 70, behavior: 'smooth' });
            }, 100);
        }
        initializeAppFunctions(); // Initialize common functions like charts and mobile menu
    }


    function initializeAppFunctions() {
        document.getElementById('current-year').textContent = new Date().getFullYear();

        const products = {
            certificates: [
                { name: "الشهادة الثلاثية ذات العائد الثابت", rate: 19.5, term: 3, payout: "شهري وربع سنوي", min: 1000, details: "استثمار آمن بعائد ثابت ومميز يُصرف شهرياً. مثال على سعر فائدة شائع." },
                { name: "شهادة الحصاد (متغيرة العائد)", rate: 23.25, term: 3, payout: "يومي وشهري", min: 1000, details: "شهادة مرتبطة بمؤشرات اقتصادية، تمنح فرصة لعائد أعلى. العائد المعروض ." },
                { name: " شهادة ادخار خماسية الدولارية", rate: 5, term: 5, payout: "شهريًا أو ربع سنويًا أو نصف سنويًا أو سنويًا", min: 100, details: "لأصحاب النظرة الاستثمارية طويلة الأجل، بعائد يصرف كل شهر. سعر الفائدة ." }
            ],
            deposits: [
                { name: "وديعة لأجل ثابت (شهرية)", details: "احفظ أموالك لمدة شهر واستفد من عائد تنافسي. (مثال: عائد 11.25% لمبلغ 2000 جنيه فأكثر)." },
                { name: "حساب التوفير المميز", details: "ادخر بمرونة مع إمكانية السحب والإيداع، واحصل على عائد دوري على رصيدك. (مثال: شرائح عائد تبدأ من 11.75%)" },
                { name: "الحساب الجاري ذو العائد اليومي", details: "لإدارة معاملاتك اليومية مع ميزة الحصول على عائد يُحتسب يومياً. (مثال: شرائح عائد تصاعدية)" }
            ],
            loans: [
                { name: "القرض الشخصي", details: "لتلبية احتياجاتك المتنوعة، تمويل يصل إلى 1.5 مليون جنيه وفترة سداد حتى 10 سنوات. (مثال: فائدة متناقصة تبدأ من 22.5%)", maxAmount: "1,500,000", maxTerm: 10, typicalRate: 22.5 },
                { name: "قرض السيارة", details: "امتلك سيارتك بتمويل يصل إلى 100% من قيمتها وإجراءات سريعة. (مثال: فائدة متناقصة، تمويل حتى 2 مليون جنيه)", maxAmount: "2,000,000", maxTerm: 8, typicalRate: 21.5 },
                { name: "التمويل العقاري", details: "حقق حلم امتلاك منزلك بشروط ميسرة وفترات سداد طويلة. (مثال: تمويل حتى 5 مليون جنيه، فائدة تنافسية)", maxAmount: "5,000,000", maxTerm: 20, typicalRate: 20.0 }
            ]
        };

        function createCard(item, type) {
            let content = `<div class="bg-white rounded-lg shadow-md p-6 flex flex-col h-full hover:shadow-xl transition-shadow duration-300">
                <h3 class="text-xl font-bold text-gray-900 mb-2">${item.name}</h3>
                <p class="text-gray-600 flex-grow mb-4">${item.details}</p>`;
            
            if (type === 'certificates') {
                content += `<div class="mt-auto pt-4 border-t border-gray-100 text-sm space-y-2">
                    <p><strong>العائد السنوي:</strong> ${item.rate}%</p>
                    <p><strong>المدة:</strong> ${item.term} سنوات</p>
                    <p><strong>دورية الصرف:</strong> ${item.payout}</p>
                    <p><strong>الحد الأدنى:</strong> ${item.min} جنية/دولار</p>
                </div>`;
            } else if (type === 'loans') {
                 content += `<div class="mt-auto pt-4 border-t border-gray-100 text-sm space-y-2">
                    <p><strong>أقصى مبلغ :</strong> ${item.maxAmount} جنيه</p>
                    <p><strong>أقصى مدة :</strong> ${item.maxTerm} سنوات</p>
                    <p><strong>سعر فائدة متناقصة :</strong> ${item.typicalRate}%</p>
                </div>`;
            }
            content += `</div>`;
            return content;
        }

        const certificatesGrid = document.getElementById('certificates-grid');
        const depositsGrid = document.getElementById('deposits-grid');
        const loansGrid = document.getElementById('loans-grid');
        
        if(certificatesGrid && certificatesGrid.innerHTML.trim() === '') products.certificates.forEach(item => { certificatesGrid.innerHTML += createCard(item, 'certificates'); });
        if(depositsGrid && depositsGrid.innerHTML.trim() === '') products.deposits.forEach(item => { depositsGrid.innerHTML += createCard(item, 'deposits'); });
        if(loansGrid && loansGrid.innerHTML.trim() === '') products.loans.forEach(item => { loansGrid.innerHTML += createCard(item, 'loans'); });
        
        const navLinks = document.querySelectorAll('header a.nav-link[href^="#"]');
        const mobileMenu = document.getElementById('mobile-menu');

        navLinks.forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const targetId = this.getAttribute('href');
                const targetElement = document.querySelector(targetId);
                if (targetElement && !targetElement.classList.contains('page-hidden')) {
                    let offset = 80; // Height of sticky header
                    if (targetId === '#hero' && !document.getElementById('hero').classList.contains('page-hidden')) offset = 0;
                    
                    const elementPosition = targetElement.getBoundingClientRect().top;
                    const offsetPosition = elementPosition + window.pageYOffset - offset;

                    window.scrollTo({
                        top: offsetPosition,
                        behavior: "smooth"
                    });
                }
                if (mobileMenu && mobileMenu.classList.contains('hidden') === false) {
                    mobileMenu.classList.add('hidden');
                }
            });
        });


        const mobileMenuButton = document.getElementById('mobile-menu-button');
        if (mobileMenuButton && mobileMenu) {
            mobileMenuButton.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });
        }
        

        let earningsChart, loanChart;

        function updateEarningsChart() {
            const certAmountEl = document.getElementById('cert-amount');
            if(!certAmountEl || certAmountEl.closest('.page-hidden')) return;

            const amount = parseFloat(certAmountEl.value) || 0;
            const rate = parseFloat(document.getElementById('cert-rate').value) || 0;
            const years = parseInt(document.getElementById('cert-years').value) || 0;

            const labels = Array.from({ length: years + 1 }, (_, i) => `سنة ${i}`);
            const data = [];
            let currentAmount = amount;
            data.push(currentAmount);

            for (let i = 0; i < years; i++) {
                currentAmount += currentAmount * (rate / 100);
                data.push(parseFloat(currentAmount.toFixed(2)));
            }

            const chartData = {
                labels: labels,
                datasets: [{
                    label: 'إجمالي المبلغ مع الأرباح (مثال)',
                    data: data,
                    borderColor: '#0e7490',
                    backgroundColor: 'rgba(14, 116, 144, 0.1)',
                    fill: true,
                    tension: 0.1
                }]
            };
            const earningsChartCanvas = document.getElementById('earningsChart');
            if (!earningsChartCanvas) return;
            const ctx = earningsChartCanvas.getContext('2d');

            if (earningsChart) {
                earningsChart.destroy(); // Destroy previous instance before creating new one
            }
            earningsChart = new Chart(ctx, { // Always create new instance
                type: 'line',
                data: chartData,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'top', labels: { font: { family: 'Cairo'}}},
                        tooltip: { mode: 'index', intersect: false, bodyFont: {family: 'Cairo'}, titleFont: {family: 'Cairo'} }
                    },
                    scales: {
                        y: { beginAtZero: false, ticks: { callback: value => `${value} ج.م`, font: {family: 'Cairo'} } },
                        x: { ticks: { font: {family: 'Cairo'} } }
                    }
                }
            });
        }

        function updateLoanCalculator() {
            const loanAmountEl = document.getElementById('loan-amount');
            if(!loanAmountEl || loanAmountEl.closest('.page-hidden')) return;

            const amount = parseFloat(loanAmountEl.value) || 0;
            const years = parseInt(document.getElementById('loan-years').value) || 0;
            const annualRate = parseFloat(document.getElementById('loan-rate').value) || 0;
            
            const monthlyRate = annualRate / 12 / 100;
            const numberOfPayments = years * 12;
            
            let monthlyPayment = 0;
            if (amount <=0 || years <= 0 || annualRate < 0) { 
                monthlyPayment = 0;
            } else if (annualRate === 0 && numberOfPayments > 0) {
                monthlyPayment = amount / numberOfPayments;
            }
            else if (monthlyRate > 0 && numberOfPayments > 0) {
                monthlyPayment = amount * monthlyRate * Math.pow(1 + monthlyRate, numberOfPayments) / (Math.pow(1 + monthlyRate, numberOfPayments) - 1);
            }

            const monthlyPaymentEl = document.getElementById('monthly-payment');
            if(monthlyPaymentEl) monthlyPaymentEl.textContent = `${monthlyPayment.toFixed(2)} جنيه`;

            const totalPayment = monthlyPayment * numberOfPayments;
            const totalInterest = (totalPayment > amount && amount > 0 && numberOfPayments > 0) ? totalPayment - amount : 0;
            const principalAmount = (amount > 0 && totalPayment >= amount && numberOfPayments > 0) ? amount : 0;


            const chartData = {
                labels: ['أصل المبلغ (مثال)', 'إجمالي الفوائد (مثال)'],
                datasets: [{
                    data: [principalAmount, totalInterest],
                    backgroundColor: ['#0e7490', '#f97316'],
                    hoverOffset: 4
                }]
            };
            const loanChartCanvas = document.getElementById('loanChart');
            if(!loanChartCanvas) return;
            const ctx = loanChartCanvas.getContext('2d');

            if (loanChart) {
                loanChart.destroy(); 
            }
            loanChart = new Chart(ctx, { 
                type: 'doughnut',
                data: chartData,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'bottom', labels: { font: { family: 'Cairo'}}},
                        tooltip: { bodyFont: {family: 'Cairo'}, titleFont: {family: 'Cairo'} }
                    }
                }
            });
        }
        
        const certAmountEl = document.getElementById('cert-amount');
        if (certAmountEl) {
            certAmountEl.addEventListener('input', updateEarningsChart);
            document.getElementById('cert-rate').addEventListener('input', updateEarningsChart);
            document.getElementById('cert-years').addEventListener('input', updateEarningsChart);
            if(!certificateCalculatorWrapper.classList.contains('page-hidden')) updateEarningsChart();
        }
        
        const loanAmountEl = document.getElementById('loan-amount');
        if (loanAmountEl) {
            loanAmountEl.addEventListener('input', updateLoanCalculator);
            document.getElementById('loan-years').addEventListener('input', updateLoanCalculator);
            document.getElementById('loan-rate').addEventListener('input', updateLoanCalculator);
            if(!loanCalculatorWrapper.classList.contains('page-hidden')) updateLoanCalculator();
        }
    }
});
</script>

</body>
</html>
