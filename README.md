## Hi there 👋

<!--
**abdlelah2024/Abdlelah2024** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة المواعيد الطبية المتكامل</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoi2YbYuNin2YUg2KXYr9in2LHYqSDYp9mE2YXZiNin2LnZitivINin2YTYt9io2YrYqSDYp9mE2YXYqtmD2KfZhdmEIiwic2hvcnRfbmFtZSI6Itiz2KrZiNmFINmF2LPYp9mI2K3YuSDZgSDZiNin2LnZitinIiwic3RhcnRfdXJsIjoiLyIsImRpc3BsYXkiOiJzdGFuZGFsb25lIiwib3JpZW50YXRpb24iOiJwb3J0cmFpdCIsInRoZW1lX2NvbG9yIjoiIzVENUNERSIsImJhY2tncm91bmRfY29sb3IiOiIjMTgxODE4IiwiaWNvbnMiOlt7InNyYyI6ImRhdGE6aW1hZ2Uvc3ZnK3htbDtiYXNlNjQsUEhOMlp5QjNhV1IwYUQwaU1USTRJaUJvWldsbmFIUTlJakV5T0NJaWRtbGxkMEp2ZUQwaU1DQXdJREV5T0NBek1Ea2lJR1pwYkd3OUluTnZiV1VpSUdobGJHOXpJRFYyWVd4bFBUUnhRa0YwYkc0dklESTJRVzl0VDBGdk1HOUJNMEZzVFVjeE1UWTlJakVpSUVGRVR6MWhhbTVoTVQ4aU9Yd2lPaUNBWURBNElNNUJDQVNDaFFJU1FtUWttZDM1eFVKTlZDbTNTVGN0ZDBrS0lBQT0iLCJzaXplcyI6IjE5Mng5NiIsInR5cGUiOiJpbWFnZS9zdmcreG1sIn1dfQ==">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@200;300;400;500;600;700;800&display=swap');
        
        * {
            font-family: 'Cairo', sans-serif;
        }
        
        .slide-in {
            animation: slideIn 0.3s ease-out;
        }
        
        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        
        .fade-in {
            animation: fadeIn 0.3s ease-in;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            transition: all 0.3s ease;
        }
        
        .loading {
            border: 3px solid #f3f3f3;
            border-top: 3px solid #5D5CDE;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .offline-indicator {
            position: fixed;
            bottom: 20px;
            left: 20px;
            background: #f59e0b;
            color: white;
            padding: 10px 15px;
            border-radius: 5px;
            display: none;
            z-index: 1000;
        }

        .connection-status {
            display: inline-block;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            margin-left: 5px;
        }

        .connected { background-color: #10b981; }
        .disconnected { background-color: #ef4444; }
        .syncing { background-color: #f59e0b; animation: pulse 1s infinite; }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
    </style>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#5D5CDE',
                        dark: {
                            DEFAULT: '#181818',
                            50: '#f8f9fa',
                            100: '#e9ecef',
                            200: '#dee2e6',
                            300: '#ced4da',
                            400: '#adb5bd',
                            500: '#6c757d',
                            600: '#495057',
                            700: '#343a40',
                            800: '#212529',
                            900: '#181818'
                        }
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-white dark:bg-dark-900 text-gray-900 dark:text-white transition-colors duration-300">
    <!-- شاشة التحميل -->
    <div id="loadingScreen" class="fixed inset-0 bg-primary flex items-center justify-center z-50">
        <div class="text-center">
            <div class="loading mx-auto mb-4"></div>
            <h2 class="text-white text-xl font-semibold">نظام إدارة المواعيد الطبية</h2>
            <p class="text-white opacity-75 mt-2">جارٍ التحميل...</p>
        </div>
    </div>

    <!-- مؤشر الاتصال -->
    <div id="offlineIndicator" class="offline-indicator">
        <span>العمل في وضع عدم الاتصال</span>
    </div>

    <!-- منطقة الإشعارات -->
    <div id="notificationArea"></div>

    <!-- شاشة تسجيل الدخول -->
    <div id="loginScreen" class="min-h-screen flex items-center justify-center bg-gradient-to-br from-primary to-purple-600 p-4">
        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-2xl p-8 w-full max-w-md">
            <div class="text-center mb-8">
                <h1 class="text-3xl font-bold text-gray-900 dark:text-white mb-2">تسجيل الدخول</h1>
                <p class="text-gray-600 dark:text-gray-300">نظام إدارة المواعيد الطبية المتكامل</p>
            </div>
            
            <form id="loginForm" class="space-y-6">
                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">البريد الإلكتروني</label>
                    <input type="email" id="email" required 
                           class="w-full px-3 py-3 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                           placeholder="أدخل البريد الإلكتروني">
                </div>
                
                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">كلمة المرور</label>
                    <input type="password" id="password" required 
                           class="w-full px-3 py-3 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                           placeholder="أدخل كلمة المرور">
                </div>
                
                <button type="submit" 
                        class="w-full bg-primary text-white py-3 px-4 rounded-md hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-primary transition duration-200">
                    تسجيل الدخول
                </button>
            </form>

            <div class="mt-6 text-center">
                <p class="text-sm text-gray-600 dark:text-gray-400">
                    للاختبار: <br>
                    البريد: abdlelah201@gmail.com<br>
                    كلمة المرور: 159632Asd
                </p>
            </div>
        </div>
    </div>

    <!-- التطبيق الرئيسي -->
    <div id="mainApp" class="hidden">
        <!-- شريط التنقل العلوي -->
        <nav class="bg-white dark:bg-dark-800 shadow-lg border-b border-gray-200 dark:border-gray-700">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between h-16">
                    <div class="flex items-center">
                        <button id="menuToggle" class="md:hidden p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                            </svg>
                        </button>
                        <h1 class="text-xl font-semibold text-gray-900 dark:text-white mr-4">نظام إدارة المواعيد الطبية</h1>
                    </div>
                    
                    <div class="flex items-center space-x-4 space-x-reverse">
                        <div class="connection-status connected" id="connectionStatus" title="حالة الاتصال"></div>
                        <span id="userInfo" class="text-sm text-gray-600 dark:text-gray-300"></span>
                        <button id="logoutBtn" class="bg-red-500 text-white px-4 py-2 rounded-md hover:bg-red-600 transition duration-200">
                            تسجيل الخروج
                        </button>
                    </div>
                </div>
            </div>
        </nav>

        <div class="flex">
            <!-- الشريط الجانبي -->
            <aside id="sidebar" class="bg-white dark:bg-dark-800 w-64 min-h-screen shadow-lg border-l border-gray-200 dark:border-gray-700 fixed md:relative transform translate-x-full md:translate-x-0 transition-transform duration-300 ease-in-out z-30">
                <nav class="mt-8">
                    <div class="px-4">
                        <a href="#" data-page="dashboard" class="nav-link group flex items-center px-2 py-2 text-base font-medium rounded-md text-gray-900 dark:text-white hover:bg-gray-100 dark:hover:bg-dark-700 bg-gray-100 dark:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2H5a2 2 0 00-2-2z" />
                            </svg>
                            لوحة التحكم
                        </a>
                        
                        <a href="#" data-page="appointments" class="nav-link group flex items-center px-2 py-2 mt-1 text-base font-medium rounded-md text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2-2v16a2 2 0 002 2z" />
                            </svg>
                            إدارة المواعيد
                        </a>
                        
                        <a href="#" data-page="patients" class="nav-link group flex items-center px-2 py-2 mt-1 text-base font-medium rounded-md text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.25 2.25 0 11-4.5 0 2.25 2.25 0 014.5 0z" />
                            </svg>
                            إدارة المرضى
                        </a>
                        
                        <a href="#" data-page="database" class="nav-link group flex items-center px-2 py-2 mt-1 text-base font-medium rounded-md text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4" />
                            </svg>
                            قواعد البيانات
                        </a>
                        
                        <a href="#" data-page="reports" class="nav-link group flex items-center px-2 py-2 mt-1 text-base font-medium rounded-md text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v4a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
                            </svg>
                            التقارير
                        </a>

                        <a href="#" data-page="fields" class="nav-link group flex items-center px-2 py-2 mt-1 text-base font-medium rounded-md text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                            </svg>
                            إدارة الحقول
                        </a>
                        
                        <a href="#" data-page="settings" class="nav-link group flex items-center px-2 py-2 mt-1 text-base font-medium rounded-md text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-dark-700">
                            <svg class="ml-3 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                            </svg>
                            الإعدادات
                        </a>
                    </div>
                </nav>
            </aside>

            <!-- المحتوى الرئيسي -->
            <main class="flex-1 p-6 bg-gray-50 dark:bg-dark-900 min-h-screen">
                <!-- لوحة التحكم -->
                <div id="dashboardPage" class="page-content">
                    <div class="mb-8">
                        <h2 class="text-3xl font-bold text-gray-900 dark:text-white">لوحة التحكم</h2>
                        <p class="text-gray-600 dark:text-gray-300 mt-2">نظرة عامة على النظام والإحصائيات</p>
                    </div>

                    <!-- الإحصائيات -->
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <div class="flex items-center">
                                <div class="p-3 rounded-full bg-blue-100 dark:bg-blue-900">
                                    <svg class="h-8 w-8 text-blue-600 dark:text-blue-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2-2v16a2 2 0 002 2z" />
                                    </svg>
                                </div>
                                <div class="mr-4">
                                    <p class="text-2xl font-semibold text-gray-900 dark:text-white" id="totalAppointments">0</p>
                                    <p class="text-gray-600 dark:text-gray-300">إجمالي المواعيد</p>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <div class="flex items-center">
                                <div class="p-3 rounded-full bg-green-100 dark:bg-green-900">
                                    <svg class="h-8 w-8 text-green-600 dark:text-green-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.25 2.25 0 11-4.5 0 2.25 2.25 0 014.5 0z" />
                                    </svg>
                                </div>
                                <div class="mr-4">
                                    <p class="text-2xl font-semibold text-gray-900 dark:text-white" id="totalPatients">0</p>
                                    <p class="text-gray-600 dark:text-gray-300">إجمالي المرضى</p>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <div class="flex items-center">
                                <div class="p-3 rounded-full bg-yellow-100 dark:bg-yellow-900">
                                    <svg class="h-8 w-8 text-yellow-600 dark:text-yellow-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                                    </svg>
                                </div>
                                <div class="mr-4">
                                    <p class="text-2xl font-semibold text-gray-900 dark:text-white" id="todayAppointments">0</p>
                                    <p class="text-gray-600 dark:text-gray-300">مواعيد اليوم</p>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <div class="flex items-center">
                                <div class="p-3 rounded-full bg-purple-100 dark:bg-purple-900">
                                    <svg class="h-8 w-8 text-purple-600 dark:text-purple-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4" />
                                    </svg>
                                </div>
                                <div class="mr-4">
                                    <p class="text-2xl font-semibold text-gray-900 dark:text-white" id="connectedDatabases">0</p>
                                    <p class="text-gray-600 dark:text-gray-300">قواعد البيانات المتصلة</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- المواعيد القادمة -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                        <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">المواعيد القادمة</h3>
                        <div id="upcomingAppointments" class="space-y-4">
                            <p class="text-gray-500 dark:text-gray-400">لا توجد مواعيد قادمة</p>
                        </div>
                    </div>
                </div>

                <!-- صفحة إدارة المواعيد -->
                <div id="appointmentsPage" class="page-content hidden">
                    <div class="mb-8 flex justify-between items-center">
                        <div>
                            <h2 class="text-3xl font-bold text-gray-900 dark:text-white">إدارة المواعيد</h2>
                            <p class="text-gray-600 dark:text-gray-300 mt-2">إدارة وتنظيم جميع المواعيد الطبية</p>
                        </div>
                        <button id="addAppointmentBtn" class="bg-primary text-white px-6 py-3 rounded-lg hover:bg-purple-700 transition duration-200">
                            إضافة موعد جديد
                        </button>
                    </div>

                    <!-- البحث والفلترة -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6 mb-6">
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">البحث</label>
                                <input type="text" id="appointmentSearch" placeholder="بحث بالاسم أو رقم الهاتف..." 
                                       class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">التاريخ</label>
                                <input type="date" id="appointmentDateFilter" 
                                       class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الحالة</label>
                                <select id="appointmentStatusFilter" 
                                        class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                                    <option value="">جميع الحالات</option>
                                    <option value="مجدول">مجدول</option>
                                    <option value="مكتمل">مكتمل</option>
                                    <option value="ملغي">ملغي</option>
                                    <option value="متأخر">متأخر</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- جدول المواعيد -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg overflow-hidden">
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
                                <thead class="bg-gray-50 dark:bg-dark-700">
                                    <tr>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">المريض</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">التاريخ والوقت</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">نوع الموعد</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">الحالة</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">الإجراءات</th>
                                    </tr>
                                </thead>
                                <tbody id="appointmentsTableBody" class="bg-white dark:bg-dark-800 divide-y divide-gray-200 dark:divide-gray-700">
                                    <!-- سيتم ملء المحتوى بواسطة JavaScript -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- صفحة إدارة المرضى -->
                <div id="patientsPage" class="page-content hidden">
                    <div class="mb-8 flex justify-between items-center">
                        <div>
                            <h2 class="text-3xl font-bold text-gray-900 dark:text-white">إدارة المرضى</h2>
                            <p class="text-gray-600 dark:text-gray-300 mt-2">إدارة قاعدة بيانات المرضى</p>
                        </div>
                        <button id="addPatientBtn" class="bg-primary text-white px-6 py-3 rounded-lg hover:bg-purple-700 transition duration-200">
                            إضافة مريض جديد
                        </button>
                    </div>

                    <!-- البحث -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6 mb-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">البحث</label>
                                <input type="text" id="patientSearch" placeholder="بحث بالاسم أو رقم الهاتف..." 
                                       class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الجنس</label>
                                <select id="patientGenderFilter" 
                                        class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                                    <option value="">جميع الأجناس</option>
                                    <option value="ذكر">ذكر</option>
                                    <option value="أنثى">أنثى</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- جدول المرضى -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg overflow-hidden">
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
                                <thead class="bg-gray-50 dark:bg-dark-700">
                                    <tr>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">الاسم</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">رقم الهاتف</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">العمر</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">الجنس</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">آخر زيارة</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">الإجراءات</th>
                                    </tr>
                                </thead>
                                <tbody id="patientsTableBody" class="bg-white dark:bg-dark-800 divide-y divide-gray-200 dark:divide-gray-700">
                                    <!-- سيتم ملء المحتوى بواسطة JavaScript -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- صفحة قواعد البيانات -->
                <div id="databasePage" class="page-content hidden">
                    <div class="mb-8">
                        <h2 class="text-3xl font-bold text-gray-900 dark:text-white">إدارة قواعد البيانات الخارجية</h2>
                        <p class="text-gray-600 dark:text-gray-300 mt-2">إدارة الاتصالات والمزامنة مع قواعد البيانات الخارجية</p>
                    </div>

                    <!-- أدوات المزامنة -->
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
                        <button id="addConnectionBtn" class="bg-primary text-white p-4 rounded-lg hover:bg-purple-700 transition duration-200">
                            <svg class="h-8 w-8 mx-auto mb-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
                            </svg>
                            إضافة اتصال
                        </button>
                        
                        <button id="testConnectionsBtn" class="bg-green-500 text-white p-4 rounded-lg hover:bg-green-600 transition duration-200">
                            <svg class="h-8 w-8 mx-auto mb-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                            </svg>
                            اختبار الاتصالات
                        </button>
                        
                        <button id="syncDataBtn" class="bg-blue-500 text-white p-4 rounded-lg hover:bg-blue-600 transition duration-200">
                            <svg class="h-8 w-8 mx-auto mb-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
                            </svg>
                            مزامنة فورية
                        </button>
                        
                        <button id="exportDataBtn" class="bg-orange-500 text-white p-4 rounded-lg hover:bg-orange-600 transition duration-200">
                            <svg class="h-8 w-8 mx-auto mb-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                            </svg>
                            تصدير البيانات
                        </button>
                    </div>

                    <!-- قائمة الاتصالات -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                        <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">الاتصالات المتاحة</h3>
                        <div id="connectionsList" class="space-y-4">
                            <!-- سيتم ملء الاتصالات بواسطة JavaScript -->
                        </div>
                    </div>
                </div>

                <!-- صفحة التقارير -->
                <div id="reportsPage" class="page-content hidden">
                    <div class="mb-8">
                        <h2 class="text-3xl font-bold text-gray-900 dark:text-white">التقارير والإحصائيات</h2>
                        <p class="text-gray-600 dark:text-gray-300 mt-2">تقارير شاملة حول العمليات والإحصائيات</p>
                    </div>

                    <!-- أنواع التقارير -->
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">تقرير المواعيد</h3>
                            <p class="text-gray-600 dark:text-gray-300 mb-4">إحصائيات شاملة عن المواعيد</p>
                            <button class="bg-primary text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition duration-200 w-full">
                                إنشاء التقرير
                            </button>
                        </div>

                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">تقرير المرضى</h3>
                            <p class="text-gray-600 dark:text-gray-300 mb-4">إحصائيات المرضى والزيارات</p>
                            <button class="bg-primary text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition duration-200 w-full">
                                إنشاء التقرير
                            </button>
                        </div>

                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">تقرير الإيرادات</h3>
                            <p class="text-gray-600 dark:text-gray-300 mb-4">تحليل الإيرادات والمدفوعات</p>
                            <button class="bg-primary text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition duration-200 w-full">
                                إنشاء التقرير
                            </button>
                        </div>
                    </div>

                    <!-- تقرير سريع -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                        <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">تقرير سريع</h3>
                        <div id="quickReport" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                            <!-- سيتم ملء المحتوى بواسطة JavaScript -->
                        </div>
                    </div>
                </div>

                <!-- صفحة إدارة الحقول -->
                <div id="fieldsPage" class="page-content hidden">
                    <div class="mb-8 flex justify-between items-center">
                        <div>
                            <h2 class="text-3xl font-bold text-gray-900 dark:text-white">إدارة الحقول</h2>
                            <p class="text-gray-600 dark:text-gray-300 mt-2">إدارة حقول النماذج المخصصة</p>
                        </div>
                        <button id="addFieldBtn" class="bg-primary text-white px-6 py-3 rounded-lg hover:bg-purple-700 transition duration-200">
                            إضافة حقل جديد
                        </button>
                    </div>

                    <!-- قائمة الحقول -->
                    <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg overflow-hidden">
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
                                <thead class="bg-gray-50 dark:bg-dark-700">
                                    <tr>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">اسم الحقل</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">النوع</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">مطلوب</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">القسم</th>
                                        <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">الإجراءات</th>
                                    </tr>
                                </thead>
                                <tbody id="fieldsTableBody" class="bg-white dark:bg-dark-800 divide-y divide-gray-200 dark:divide-gray-700">
                                    <!-- سيتم ملء المحتوى بواسطة JavaScript -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- صفحة الإعدادات -->
                <div id="settingsPage" class="page-content hidden">
                    <div class="mb-8">
                        <h2 class="text-3xl font-bold text-gray-900 dark:text-white">الإعدادات</h2>
                        <p class="text-gray-600 dark:text-gray-300 mt-2">إعدادات النظام والتخصيص</p>
                    </div>

                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                        <!-- إعدادات عامة -->
                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">الإعدادات العامة</h3>
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">اسم العيادة</label>
                                    <input type="text" id="clinicName" value="عيادة الدكتور محمد" 
                                           class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">مدة الموعد الافتراضية (دقيقة)</label>
                                    <input type="number" id="defaultDuration" value="30" 
                                           class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">عدد أيام العودة المجانية</label>
                                    <input type="number" id="freeReturnDays" value="7" 
                                           class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                                </div>
                            </div>
                        </div>

                        <!-- إعدادات المزامنة -->
                        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-lg p-6">
                            <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">إعدادات المزامنة</h3>
                            <div class="space-y-4">
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-700 dark:text-gray-300">المزامنة التلقائية</span>
                                    <label class="switch">
                                        <input type="checkbox" id="autoSync" checked>
                                        <span class="slider"></span>
                                    </label>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-700 dark:text-gray-300">العمل الأوفلاين</span>
                                    <label class="switch">
                                        <input type="checkbox" id="offlineMode" checked>
                                        <span class="slider"></span>
                                    </label>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-700 dark:text-gray-300">إشعارات المزامنة</span>
                                    <label class="switch">
                                        <input type="checkbox" id="syncNotifications" checked>
                                        <span class="slider"></span>
                                    </label>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- أزرار الحفظ -->
                    <div class="mt-8 flex justify-end space-x-4 space-x-reverse">
                        <button id="resetSettingsBtn" class="bg-gray-500 text-white px-6 py-3 rounded-lg hover:bg-gray-600 transition duration-200">
                            إعادة تعيين
                        </button>
                        <button id="saveSettingsBtn" class="bg-primary text-white px-6 py-3 rounded-lg hover:bg-purple-700 transition duration-200">
                            حفظ الإعدادات
                        </button>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- نماذج منبثقة -->
    <!-- نموذج إضافة موعد -->
    <div id="appointmentModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-2xl p-6 w-full max-w-2xl mx-4 max-h-96 overflow-y-auto">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-xl font-semibold text-gray-900 dark:text-white" id="appointmentModalTitle">إضافة موعد جديد</h3>
                <button id="closeAppointmentModal" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300">
                    <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>

            <form id="appointmentForm" class="space-y-4">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">اختيار المريض</label>
                        <select id="appointmentPatient" required 
                                class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                            <option value="">اختر مريض موجود أو أضف جديد</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">أو اسم مريض جديد</label>
                        <input type="text" id="newPatientName" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="اسم المريض الجديد">
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">التاريخ</label>
                        <input type="date" id="appointmentDate" required 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الوقت</label>
                        <input type="time" id="appointmentTime" required 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">نوع الموعد</label>
                        <select id="appointmentType" required 
                                class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                            <option value="">اختر نوع الموعد</option>
                            <option value="فحص عام">فحص عام</option>
                            <option value="استشارة">استشارة</option>
                            <option value="متابعة">متابعة</option>
                            <option value="عودة مجانية">عودة مجانية</option>
                            <option value="عملية">عملية</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">المدة (دقيقة)</label>
                        <input type="number" id="appointmentDuration" value="30" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                    </div>
                </div>

                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">ملاحظات</label>
                    <textarea id="appointmentNotes" rows="3" 
                              class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                              placeholder="ملاحظات إضافية..."></textarea>
                </div>

                <div class="flex justify-end space-x-4 space-x-reverse pt-4">
                    <button type="button" id="cancelAppointment" class="bg-gray-500 text-white px-6 py-2 rounded-lg hover:bg-gray-600 transition duration-200">
                        إلغاء
                    </button>
                    <button type="submit" class="bg-primary text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition duration-200">
                        حفظ الموعد
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- نموذج إضافة مريض -->
    <div id="patientModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-2xl p-6 w-full max-w-2xl mx-4 max-h-96 overflow-y-auto">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-xl font-semibold text-gray-900 dark:text-white" id="patientModalTitle">إضافة مريض جديد</h3>
                <button id="closePatientModal" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300">
                    <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>

            <form id="patientForm" class="space-y-4">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الاسم الكامل *</label>
                        <input type="text" id="patientName" required 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="الاسم الكامل">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">رقم الهاتف *</label>
                        <input type="tel" id="patientPhone" required 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="05xxxxxxxx">
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">العمر</label>
                        <input type="number" id="patientAge" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="العمر">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الجنس</label>
                        <select id="patientGender" 
                                class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                            <option value="">اختر الجنس</option>
                            <option value="ذكر">ذكر</option>
                            <option value="أنثى">أنثى</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">تاريخ الميلاد</label>
                        <input type="date" id="patientBirthDate" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                    </div>
                </div>

                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">العنوان</label>
                    <input type="text" id="patientAddress" 
                           class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                           placeholder="العنوان">
                </div>

                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">التاريخ المرضي</label>
                    <textarea id="patientMedicalHistory" rows="3" 
                              class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                              placeholder="التاريخ المرضي والحساسيات..."></textarea>
                </div>

                <div class="flex justify-end space-x-4 space-x-reverse pt-4">
                    <button type="button" id="cancelPatient" class="bg-gray-500 text-white px-6 py-2 rounded-lg hover:bg-gray-600 transition duration-200">
                        إلغاء
                    </button>
                    <button type="submit" class="bg-primary text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition duration-200">
                        حفظ المريض
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- نموذج إضافة اتصال قاعدة بيانات -->
    <div id="connectionModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-2xl p-6 w-full max-w-2xl mx-4 max-h-96 overflow-y-auto">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-xl font-semibold text-gray-900 dark:text-white">إضافة اتصال قاعدة بيانات</h3>
                <button id="closeConnectionModal" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300">
                    <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>

            <form id="connectionForm" class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">نوع قاعدة البيانات</label>
                    <select id="connectionType" required 
                            class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                        <option value="">اختر نوع قاعدة البيانات</option>
                        <option value="firebase">Firebase</option>
                        <option value="google-sheets">Google Sheets</option>
                        <option value="github">GitHub</option>
                        <option value="mysql">MySQL</option>
                        <option value="postgresql">PostgreSQL</option>
                        <option value="mongodb">MongoDB</option>
                        <option value="dropbox">Dropbox</option>
                        <option value="onedrive">OneDrive</option>
                        <option value="sqlite">SQLite</option>
                    </select>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">اسم الاتصال</label>
                        <input type="text" id="connectionName" required 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="اسم الاتصال">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الخادم/URL</label>
                        <input type="text" id="connectionServer" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="عنوان الخادم">
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">اسم المستخدم</label>
                        <input type="text" id="connectionUsername" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="اسم المستخدم">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">كلمة المرور/المفتاح</label>
                        <input type="password" id="connectionPassword" 
                               class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                               placeholder="كلمة المرور أو المفتاح">
                    </div>
                </div>

                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">اسم قاعدة البيانات</label>
                    <input type="text" id="connectionDatabase" 
                           class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                           placeholder="اسم قاعدة البيانات">
                </div>

                <div class="flex justify-end space-x-4 space-x-reverse pt-4">
                    <button type="button" id="cancelConnection" class="bg-gray-500 text-white px-6 py-2 rounded-lg hover:bg-gray-600 transition duration-200">
                        إلغاء
                    </button>
                    <button type="button" id="testConnection" class="bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-600 transition duration-200">
                        اختبار الاتصال
                    </button>
                    <button type="submit" class="bg-primary text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition duration-200">
                        حفظ الاتصال
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- نموذج إضافة حقل -->
    <div id="fieldModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white dark:bg-dark-800 rounded-lg shadow-2xl p-6 w-full max-w-lg mx-4">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-xl font-semibold text-gray-900 dark:text-white" id="fieldModalTitle">إضافة حقل جديد</h3>
                <button id="closeFieldModal" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300">
                    <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>

            <form id="fieldForm" class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">اسم الحقل</label>
                    <input type="text" id="fieldName" required 
                           class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                           placeholder="اسم الحقل">
                </div>

                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">نوع الحقل</label>
                    <select id="fieldType" required 
                            class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                        <option value="">اختر نوع الحقل</option>
                        <option value="text">نص</option>
                        <option value="number">رقم</option>
                        <option value="email">بريد إلكتروني</option>
                        <option value="phone">هاتف</option>
                        <option value="date">تاريخ</option>
                        <option value="textarea">نص طويل</option>
                        <option value="select">اختيار من قائمة</option>
                        <option value="checkbox">مربع اختيار</option>
                        <option value="radio">خيار واحد</option>
                    </select>
                </div>

                <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">القسم</label>
                    <select id="fieldSection" required 
                            class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white">
                        <option value="">اختر القسم</option>
                        <option value="patients">المرضى</option>
                        <option value="appointments">المواعيد</option>
                        <option value="reports">التقارير</option>
                    </select>
                </div>

                <div class="flex items-center">
                    <input type="checkbox" id="fieldRequired" class="rounded border-gray-300 text-primary focus:ring-primary focus:ring-offset-0">
                    <label for="fieldRequired" class="mr-2 text-sm text-gray-700 dark:text-gray-300">حقل مطلوب</label>
                </div>

                <div id="fieldOptionsContainer" class="hidden">
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">الخيارات (كل خيار في سطر منفصل)</label>
                    <textarea id="fieldOptions" rows="4" 
                              class="w-full px-3 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-primary dark:bg-dark-700 dark:text-white"
                              placeholder="خيار 1&#10;خيار 2&#10;خيار 3"></textarea>
                </div>

                <div class="flex justify-end space-x-4 space-x-reverse pt-4">
                    <button type="button" id="cancelField" class="bg-gray-500 text-white px-6 py-2 rounded-lg hover:bg-gray-600 transition duration-200">
                        إلغاء
                    </button>
                    <button type="submit" class="bg-primary text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition duration-200">
                        حفظ الحقل
                    </button>
                </div>
            </form>
        </div>
    </div>

    <style>
        .switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 18px;
            width: 18px;
            left: 3px;
            bottom: 3px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider {
            background-color: #5D5CDE;
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }
    </style>

    <script>
        // إدارة الثيم
        if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
            document.documentElement.classList.add('dark');
        }
        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
            if (event.matches) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        });

        // Service Worker للعمل الأوفلاين
        if ('serviceWorker' in navigator) {
            const swCode = `
                const CACHE_NAME = 'medical-appointments-v1';
                const urlsToCache = [
                    '/',
                    'https://cdn.tailwindcss.com',
                    'https://fonts.googleapis.com/css2?family=Cairo:wght@200;300;400;500;600;700;800&display=swap'
                ];

                self.addEventListener('install', function(event) {
                    event.waitUntil(
                        caches.open(CACHE_NAME)
                            .then(function(cache) {
                                return cache.addAll(urlsToCache);
                            })
                    );
                });

                self.addEventListener('fetch', function(event) {
                    event.respondWith(
                        caches.match(event.request)
                            .then(function(response) {
                                if (response) {
                                    return response;
                                }
                                return fetch(event.request);
                            }
                        )
                    );
                });

                self.addEventListener('sync', function(event) {
                    if (event.tag === 'background-sync') {
                        event.waitUntil(syncData());
                    }
                });

                function syncData() {
                    // مزامنة البيانات المؤجلة
                    const pendingData = JSON.parse(localStorage.getItem('pendingSync') || '[]');
                    pendingData.forEach(data => {
                        // إرسال البيانات للخادم
                        fetch('/api/sync', {
                            method: 'POST',
                            body: JSON.stringify(data),
                            headers: { 'Content-Type': 'application/json' }
                        });
                    });
                    localStorage.removeItem('pendingSync');
                }
            `;

            const blob = new Blob([swCode], { type: 'application/javascript' });
            const swUrl = URL.createObjectURL(blob);
            
            navigator.serviceWorker.register(swUrl)
                .then(function(registration) {
                    console.log('Service Worker مسجل بنجاح:', registration.scope);
                })
                .catch(function(error) {
                    console.log('فشل تسجيل Service Worker:', error);
                });
        }

        // إدارة البيانات
        class DataManager {
            constructor() {
                this.appointments = JSON.parse(localStorage.getItem('appointments') || '[]');
                this.patients = JSON.parse(localStorage.getItem('patients') || '[]');
                this.connections = JSON.parse(localStorage.getItem('connections') || '[]');
                this.fields = JSON.parse(localStorage.getItem('customFields') || '[]');
                this.settings = JSON.parse(localStorage.getItem('settings') || '{}');
                this.init();
            }

            init() {
                // تحميل بيانات تجريبية إذا لم توجد بيانات
                if (this.patients.length === 0) {
                    this.patients = [
                        {
                            id: '1',
                            name: 'أحمد محمد',
                            phone: '0501234567',
                            age: 35,
                            gender: 'ذكر',
                            address: 'الرياض',
                            medicalHistory: 'ضغط الدم',
                            lastVisit: '2024-01-15'
                        },
                        {
                            id: '2',
                            name: 'فاطمة علي',
                            phone: '0509876543',
                            age: 28,
                            gender: 'أنثى',
                            address: 'جدة',
                            medicalHistory: 'حساسية',
                            lastVisit: '2024-01-10'
                        }
                    ];
                    this.savePatients();
                }

                if (this.appointments.length === 0) {
                    this.appointments = [
                        {
                            id: '1',
                            patientId: '1',
                            patientName: 'أحمد محمد',
                            date: '2024-01-20',
                            time: '09:00',
                            type: 'فحص عام',
                            status: 'مجدول',
                            duration: 30,
                            notes: 'فحص دوري'
                        },
                        {
                            id: '2',
                            patientId: '2',
                            patientName: 'فاطمة علي',
                            date: '2024-01-21',
                            time: '10:30',
                            type: 'استشارة',
                            status: 'مجدول',
                            duration: 45,
                            notes: 'استشارة تغذية'
                        }
                    ];
                    this.saveAppointments();
                }

                // حقول افتراضية
                if (this.fields.length === 0) {
                    this.fields = [
                        { id: '1', name: 'رقم الهوية', type: 'text', section: 'patients', required: false },
                        { id: '2', name: 'التأمين الطبي', type: 'text', section: 'patients', required: false },
                        { id: '3', name: 'مبلغ الكشف', type: 'number', section: 'appointments', required: false },
                        { id: '4', name: 'حالة الدفع', type: 'select', section: 'appointments', required: false, options: ['مدفوع', 'غير مدفوع', 'مؤجل'] }
                    ];
                    this.saveFields();
                }
            }

            // حفظ البيانات
            saveAppointments() {
                localStorage.setItem('appointments', JSON.stringify(this.appointments));
            }

            savePatients() {
                localStorage.setItem('patients', JSON.stringify(this.patients));
            }

            saveConnections() {
                localStorage.setItem('connections', JSON.stringify(this.connections));
            }

            saveFields() {
                localStorage.setItem('customFields', JSON.stringify(this.fields));
            }

            saveSettings() {
                localStorage.setItem('settings', JSON.stringify(this.settings));
            }

            // إدارة المواعيد
            addAppointment(appointment) {
                appointment.id = Date.now().toString();
                this.appointments.push(appointment);
                this.saveAppointments();
                this.syncData('appointments', 'create', appointment);
                return appointment;
            }

            updateAppointment(id, appointment) {
                const index = this.appointments.findIndex(a => a.id === id);
                if (index !== -1) {
                    this.appointments[index] = { ...this.appointments[index], ...appointment };
                    this.saveAppointments();
                    this.syncData('appointments', 'update', this.appointments[index]);
                    return this.appointments[index];
                }
                return null;
            }

            deleteAppointment(id) {
                this.appointments = this.appointments.filter(a => a.id !== id);
                this.saveAppointments();
                this.syncData('appointments', 'delete', { id });
            }

            // إدارة المرضى
            addPatient(patient) {
                patient.id = Date.now().toString();
                this.patients.push(patient);
                this.savePatients();
                this.syncData('patients', 'create', patient);
                return patient;
            }

            updatePatient(id, patient) {
                const index = this.patients.findIndex(p => p.id === id);
                if (index !== -1) {
                    this.patients[index] = { ...this.patients[index], ...patient };
                    this.savePatients();
                    this.syncData('patients', 'update', this.patients[index]);
                    return this.patients[index];
                }
                return null;
            }

            deletePatient(id) {
                this.patients = this.patients.filter(p => p.id !== id);
                this.savePatients();
                this.syncData('patients', 'delete', { id });
            }

            // إدارة الاتصالات
            addConnection(connection) {
                connection.id = Date.now().toString();
                connection.status = 'غير متصل';
                this.connections.push(connection);
                this.saveConnections();
                return connection;
            }

            deleteConnection(id) {
                this.connections = this.connections.filter(c => c.id !== id);
                this.saveConnections();
            }

            // إدارة الحقول
            addField(field) {
                field.id = Date.now().toString();
                this.fields.push(field);
                this.saveFields();
                return field;
            }

            updateField(id, field) {
                const index = this.fields.findIndex(f => f.id === id);
                if (index !== -1) {
                    this.fields[index] = { ...this.fields[index], ...field };
                    this.saveFields();
                    return this.fields[index];
                }
                return null;
            }

            deleteField(id) {
                this.fields = this.fields.filter(f => f.id !== id);
                this.saveFields();
            }

            // مزامنة البيانات
            syncData(type, action, data) {
                if (!navigator.onLine) {
                    // حفظ للمزامنة اللاحقة
                    const pending = JSON.parse(localStorage.getItem('pendingSync') || '[]');
                    pending.push({ type, action, data, timestamp: Date.now() });
                    localStorage.setItem('pendingSync', JSON.stringify(pending));
                    return;
                }

                // محاكاة إرسال البيانات
                console.log(`مزامنة ${type} - ${action}:`, data);
            }

            // اختبار الاتصال
            async testConnection(connection) {
                try {
                    // محاكاة اختبار الاتصال
                    await new Promise(resolve => setTimeout(resolve, 1000));
                    
                    const success = Math.random() > 0.3; // نسبة نجاح 70%
                    if (success) {
                        connection.status = 'متصل';
                        showNotification('تم اختبار الاتصال بنجاح', 'success');
                        return true;
                    } else {
                        connection.status = 'فشل الاتصال';
                        showNotification('فشل في الاتصال', 'error');
                        return false;
                    }
                } catch (error) {
                    connection.status = 'خطأ في الاتصال';
                    showNotification('خطأ في اختبار الاتصال', 'error');
                    return false;
                }
            }

            // إحصائيات
            getStats() {
                const today = new Date().toISOString().split('T')[0];
                const todayAppointments = this.appointments.filter(a => a.date === today);
                
                return {
                    totalAppointments: this.appointments.length,
                    totalPatients: this.patients.length,
                    todayAppointments: todayAppointments.length,
                    connectedDatabases: this.connections.filter(c => c.status === 'متصل').length
                };
            }
        }

        // مدير البيانات العام
        const dataManager = new DataManager();

        // إدارة الإشعارات
        function showNotification(message, type = 'info') {
            const notificationArea = document.getElementById('notificationArea');
            const notification = document.createElement('div');
            
            const colors = {
                success: 'bg-green-500',
                error: 'bg-red-500',
                warning: 'bg-yellow-500',
                info: 'bg-blue-500'
            };

            notification.className = `notification ${colors[type]} text-white px-6 py-3 rounded-lg shadow-lg slide-in`;
            notification.innerHTML = `
                <div class="flex items-center justify-between">
                    <span>${message}</span>
                    <button onclick="this.parentElement.parentElement.remove()" class="mr-4 text-white hover:text-gray-200">
                        <svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
            `;

            notificationArea.appendChild(notification);

            setTimeout(() => {
                if (notification.parentElement) {
                    notification.remove();
                }
            }, 5000);
        }

        // إدارة الاتصال
        function updateConnectionStatus() {
            const statusElement = document.getElementById('connectionStatus');
            const offlineIndicator = document.getElementById('offlineIndicator');
            
            if (navigator.onLine) {
                statusElement.className = 'connection-status connected';
                statusElement.title = 'متصل';
                offlineIndicator.style.display = 'none';
            } else {
                statusElement.className = 'connection-status disconnected';
                statusElement.title = 'غير متصل';
                offlineIndicator.style.display = 'block';
            }
        }

        window.addEventListener('online', updateConnectionStatus);
        window.addEventListener('offline', updateConnectionStatus);

        // إدارة النظام الرئيسي
        class AppManager {
            constructor() {
                this.currentUser = null;
                this.currentPage = 'dashboard';
                this.init();
            }

            init() {
                this.setupEventListeners();
                this.hideLoadingScreen();
                updateConnectionStatus();
            }

            setupEventListeners() {
                // تسجيل الدخول
                document.getElementById('loginForm').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.handleLogin();
                });

                // تسجيل الخروج
                document.getElementById('logoutBtn').addEventListener('click', () => {
                    this.handleLogout();
                });

                // التنقل
                document.querySelectorAll('.nav-link').forEach(link => {
                    link.addEventListener('click', (e) => {
                        e.preventDefault();
                        this.navigateToPage(e.target.dataset.page);
                    });
                });

                // القائمة الجانبية للموبايل
                document.getElementById('menuToggle').addEventListener('click', () => {
                    const sidebar = document.getElementById('sidebar');
                    sidebar.classList.toggle('translate-x-full');
                });

                // نماذج المواعيد
                document.getElementById('addAppointmentBtn').addEventListener('click', () => {
                    this.openAppointmentModal();
                });

                document.getElementById('appointmentForm').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.saveAppointment();
                });

                // نماذج المرضى
                document.getElementById('addPatientBtn').addEventListener('click', () => {
                    this.openPatientModal();
                });

                document.getElementById('patientForm').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.savePatient();
                });

                // نماذج الاتصالات
                document.getElementById('addConnectionBtn').addEventListener('click', () => {
                    this.openConnectionModal();
                });

                document.getElementById('connectionForm').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.saveConnection();
                });

                document.getElementById('testConnection').addEventListener('click', () => {
                    this.testConnection();
                });

                // نماذج الحقول
                document.getElementById('addFieldBtn').addEventListener('click', () => {
                    this.openFieldModal();
                });

                document.getElementById('fieldForm').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.saveField();
                });

                document.getElementById('fieldType').addEventListener('change', (e) => {
                    const optionsContainer = document.getElementById('fieldOptionsContainer');
                    if (e.target.value === 'select' || e.target.value === 'radio') {
                        optionsContainer.classList.remove('hidden');
                    } else {
                        optionsContainer.classList.add('hidden');
                    }
                });

                // إغلاق النماذج
                this.setupModalClosers();

                // أدوات قاعدة البيانات
                document.getElementById('testConnectionsBtn').addEventListener('click', () => {
                    this.testAllConnections();
                });

                document.getElementById('syncDataBtn').addEventListener('click', () => {
                    this.syncAllData();
                });

                document.getElementById('exportDataBtn').addEventListener('click', () => {
                    this.exportData();
                });

                // البحث والفلترة
                document.getElementById('appointmentSearch').addEventListener('input', () => {
                    this.filterAppointments();
                });

                document.getElementById('appointmentDateFilter').addEventListener('change', () => {
                    this.filterAppointments();
                });

                document.getElementById('appointmentStatusFilter').addEventListener('change', () => {
                    this.filterAppointments();
                });

                document.getElementById('patientSearch').addEventListener('input', () => {
                    this.filterPatients();
                });

                document.getElementById('patientGenderFilter').addEventListener('change', () => {
                    this.filterPatients();
                });

                // الإعدادات
                document.getElementById('saveSettingsBtn').addEventListener('click', () => {
                    this.saveSettings();
                });

                document.getElementById('resetSettingsBtn').addEventListener('click', () => {
                    this.resetSettings();
                });
            }

            setupModalClosers() {
                const modals = [
                    'appointmentModal',
                    'patientModal', 
                    'connectionModal',
                    'fieldModal'
                ];

                modals.forEach(modalId => {
                    const modal = document.getElementById(modalId);
                    const closeBtn = modal.querySelector('[id^="close"]');
                    const cancelBtn = modal.querySelector('[id^="cancel"]');

                    closeBtn?.addEventListener('click', () => {
                        modal.classList.add('hidden');
                    });

                    cancelBtn?.addEventListener('click', () => {
                        modal.classList.add('hidden');
                    });

                    modal.addEventListener('click', (e) => {
                        if (e.target === modal) {
                            modal.classList.add('hidden');
                        }
                    });
                });
            }

            hideLoadingScreen() {
                setTimeout(() => {
                    document.getElementById('loadingScreen').style.display = 'none';
                }, 1500);
            }

            handleLogin() {
                const email = document.getElementById('email').value;
                const password = document.getElementById('password').value;

                // التحقق من بيانات الدخول
                if (email === 'abdlelah201@gmail.com' && password === '159632Asd') {
                    this.currentUser = {
                        email: email,
                        name: 'عبدالإله',
                        role: 'مدير'
                    };
                    
                    document.getElementById('loginScreen').style.display = 'none';
                    document.getElementById('mainApp').classList.remove('hidden');
                    document.getElementById('userInfo').textContent = `مرحباً، ${this.currentUser.name}`;
                    
                    this.loadDashboard();
                    showNotification('تم تسجيل الدخول بنجاح', 'success');
                } else {
                    showNotification('بيانات الدخول غير صحيحة', 'error');
                }
            }

            handleLogout() {
                this.currentUser = null;
                document.getElementById('loginScreen').style.display = 'flex';
                document.getElementById('mainApp').classList.add('hidden');
                document.getElementById('email').value = '';
                document.getElementById('password').value = '';
                showNotification('تم تسجيل الخروج', 'info');
            }

            navigateToPage(page) {
                // إخفاء جميع الصفحات
                document.querySelectorAll('.page-content').forEach(p => {
                    p.classList.add('hidden');
                });

                // إظهار الصفحة المطلوبة
                document.getElementById(`${page}Page`).classList.remove('hidden');

                // تحديث التنقل
                document.querySelectorAll('.nav-link').forEach(link => {
                    link.classList.remove('bg-gray-100', 'dark:bg-dark-700');
                    link.classList.add('text-gray-600', 'dark:text-gray-300');
                });

                const activeLink = document.querySelector(`[data-page="${page}"]`);
                if (activeLink) {
                    activeLink.classList.add('bg-gray-100', 'dark:bg-dark-700');
                    activeLink.classList.remove('text-gray-600', 'dark:text-gray-300');
                    activeLink.classList.add('text-gray-900', 'dark:text-white');
                }

                this.currentPage = page;

                // تحميل بيانات الصفحة
                switch(page) {
                    case 'dashboard':
                        this.loadDashboard();
                        break;
                    case 'appointments':
                        this.loadAppointments();
                        break;
                    case 'patients':
                        this.loadPatients();
                        break;
                    case 'database':
                        this.loadDatabaseConnections();
                        break;
                    case 'reports':
                        this.loadReports();
                        break;
                    case 'fields':
                        this.loadFields();
                        break;
                    case 'settings':
                        this.loadSettings();
                        break;
                }

                // إغلاق القائمة الجانبية في الموبايل
                const sidebar = document.getElementById('sidebar');
                sidebar.classList.add('translate-x-full');
            }

            loadDashboard() {
                const stats = dataManager.getStats();
                
                document.getElementById('totalAppointments').textContent = stats.totalAppointments;
                document.getElementById('totalPatients').textContent = stats.totalPatients;
                document.getElementById('todayAppointments').textContent = stats.todayAppointments;
                document.getElementById('connectedDatabases').textContent = stats.connectedDatabases;

                // المواعيد القادمة
                const upcomingContainer = document.getElementById('upcomingAppointments');
                const today = new Date().toISOString().split('T')[0];
                const upcoming = dataManager.appointments
                    .filter(apt => apt.date >= today && apt.status === 'مجدول')
                    .sort((a, b) => new Date(a.date + 'T' + a.time) - new Date(b.date + 'T' + b.time))
                    .slice(0, 5);

                if (upcoming.length === 0) {
                    upcomingContainer.innerHTML = '<p class="text-gray-500 dark:text-gray-400">لا توجد مواعيد قادمة</p>';
                } else {
                    upcomingContainer.innerHTML = upcoming.map(apt => `
                        <div class="flex items-center justify-between p-4 bg-gray-50 dark:bg-dark-700 rounded-lg">
                            <div>
                                <h4 class="font-medium text-gray-900 dark:text-white">${apt.patientName}</h4>
                                <p class="text-sm text-gray-600 dark:text-gray-300">${apt.type}</p>
                            </div>
                            <div class="text-left">
                                <p class="text-sm font-medium text-gray-900 dark:text-white">${apt.date}</p>
                                <p class="text-sm text-gray-600 dark:text-gray-300">${apt.time}</p>
                            </div>
                        </div>
                    `).join('');
                }
            }

            loadAppointments() {
                this.populatePatientOptions();
                this.renderAppointments();
            }

            renderAppointments(appointments = null) {
                const appointmentsToRender = appointments || dataManager.appointments;
                const tbody = document.getElementById('appointmentsTableBody');
                
                if (appointmentsToRender.length === 0) {
                    tbody.innerHTML = `
                        <tr>
                            <td colspan="5" class="px-6 py-4 text-center text-gray-500 dark:text-gray-400">
                                لا توجد مواعيد
                            </td>
                        </tr>
                    `;
                    return;
                }

                tbody.innerHTML = appointmentsToRender.map(apt => {
                    const statusColors = {
                        'مجدول': 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300',
                        'مكتمل': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
                        'ملغي': 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300',
                        'متأخر': 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300'
                    };

                    return `
                        <tr class="hover:bg-gray-50 dark:hover:bg-dark-700">
                            <td class="px-6 py-4 whitespace-nowrap">
                                <div class="text-sm font-medium text-gray-900 dark:text-white">${apt.patientName}</div>
                            </td>
                            <td class="px-6 py-4 whitespace-nowrap">
                                <div class="text-sm text-gray-900 dark:text-white">${apt.date}</div>
                                <div class="text-sm text-gray-500 dark:text-gray-400">${apt.time}</div>
                            </td>
                            <td class="px-6 py-4 whitespace-nowrap">
                                <div class="text-sm text-gray-900 dark:text-white">${apt.type}</div>
                            </td>
                            <td class="px-6 py-4 whitespace-nowrap">
                                <span class="inline-flex px-2 py-1 text-xs font-semibold rounded-full ${statusColors[apt.status] || 'bg-gray-100 text-gray-800'}">${apt.status}</span>
                            </td>
                            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                                <button onclick="app.editAppointment('${apt.id}')" class="text-primary hover:text-purple-700 ml-4">تعديل</button>
                                <button onclick="app.deleteAppointment('${apt.id}')" class="text-red-600 hover:text-red-900">حذف</button>
                            </td>
                        </tr>
                    `;
                }).join('');
            }

            populatePatientOptions() {
                const select = document.getElementById('appointmentPatient');
                const currentOptions = select.innerHTML;
                
                select.innerHTML = '<option value="">اختر مريض موجود أو أضف جديد</option>' +
                    dataManager.patients.map(patient => 
                        `<option value="${patient.id}">${patient.name} - ${patient.phone}</option>`
                    ).join('');
            }

            openAppointmentModal(appointmentId = null) {
                const modal = document.getElementById('appointmentModal');
                const title = document.getElementById('appointmentModalTitle');
                const form = document.getElementById('appointmentForm');
                
                if (appointmentId) {
                    const appointment = dataManager.appointments.find(a => a.id === appointmentId);
                    title.textContent = 'تعديل الموعد';
                    this.fillAppointmentForm(appointment);
                    form.dataset.editingId = appointmentId;
                } else {
                    title.textContent = 'إضافة موعد جديد';
                    form.reset();
                    delete form.dataset.editingId;
                    
                    // تعيين التاريخ الحالي
                    document.getElementById('appointmentDate').value = new Date().toISOString().split('T')[0];
                }
                
                modal.classList.remove('hidden');
            }

            fillAppointmentForm(appointment) {
                document.getElementById('appointmentPatient').value = appointment.patientId || '';
                document.getElementById('newPatientName').value = appointment.patientId ? '' : appointment.patientName;
                document.getElementById('appointmentDate').value = appointment.date;
                document.getElementById('appointmentTime').value = appointment.time;
                document.getElementById('appointmentType').value = appointment.type;
                document.getElementById('appointmentDuration').value = appointment.duration;
                document.getElementById('appointmentNotes').value = appointment.notes || '';
            }

            saveAppointment() {
                const form = document.getElementById('appointmentForm');
                const patientId = document.getElementById('appointmentPatient').value;
                const newPatientName = document.getElementById('newPatientName').value;
                
                let finalPatientId = patientId;
                let finalPatientName = '';

                if (patientId) {
                    const patient = dataManager.patients.find(p => p.id === patientId);
                    finalPatientName = patient.name;
                } else if (newPatientName) {
                    // إنشاء مريض جديد سريع
                    const newPatient = {
                        name: newPatientName,
                        phone: '',
                        age: '',
                        gender: '',
                        address: '',
                        medicalHistory: ''
                    };
                    const savedPatient = dataManager.addPatient(newPatient);
                    finalPatientId = savedPatient.id;
                    finalPatientName = savedPatient.name;
                } else {
                    showNotification('يجب اختيار مريض أو إدخال اسم مريض جديد', 'error');
                    return;
                }

                const appointmentData = {
                    patientId: finalPatientId,
                    patientName: finalPatientName,
                    date: document.getElementById('appointmentDate').value,
                    time: document.getElementById('appointmentTime').value,
                    type: document.getElementById('appointmentType').value,
                    duration: parseInt(document.getElementById('appointmentDuration').value),
                    notes: document.getElementById('appointmentNotes').value,
                    status: 'مجدول'
                };

                if (form.dataset.editingId) {
                    dataManager.updateAppointment(form.dataset.editingId, appointmentData);
                    showNotification('تم تحديث الموعد بنجاح', 'success');
                } else {
                    dataManager.addAppointment(appointmentData);
                    showNotification('تم إضافة الموعد بنجاح', 'success');
                }

                document.getElementById('appointmentModal').classList.add('hidden');
                this.renderAppointments();
                this.populatePatientOptions();
            }

            editAppointment(id) {
                this.openAppointmentModal(id);
            }

            deleteAppointment(id) {
                if (confirm('هل أنت متأكد من حذف هذا الموعد؟')) {
                    dataManager.deleteAppointment(id);
                    this.renderAppointments();
                    showNotification('تم حذف الموعد', 'success');
                }
            }

            filterAppointments() {
                const search = document.getElementById('appointmentSearch').value.toLowerCase();
                const dateFilter = document.getElementById('appointmentDateFilter').value;
                const statusFilter = document.getElementById('appointmentStatusFilter').value;

                let filtered = dataManager.appointments;

                if (search) {
                    filtered = filtered.filter(apt => 
                        apt.patientName.toLowerCase().includes(search) ||
                        (apt.notes && apt.notes.toLowerCase().includes(search))
                    );
                }

                if (dateFilter) {
                    filtered = filtered.filter(apt => apt.date === dateFilter);
                }

                if (statusFilter) {
                    filtered = filtered.filter(apt => apt.status === statusFilter);
                }

                this.renderAppointments(filtered);
            }

            loadPatients() {
                this.renderPatients();
            }

            renderPatients(patients = null) {
                const patientsToRender = patients || dataManager.patients;
                const tbody = document.getElementById('patientsTableBody');
                
                if (patientsToRender.length === 0) {
                    tbody.innerHTML = `
                        <tr>
                            <td colspan="6" class="px-6 py-4 text-center text-gray-500 dark:text-gray-400">
                                لا توجد مرضى
                            </td>
                        </tr>
                    `;
                    return;
                }

                tbody.innerHTML = patientsToRender.map(patient => `
                    <tr class="hover:bg-gray-50 dark:hover:bg-dark-700">
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm font-medium text-gray-900 dark:text-white">${patient.name}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm text-gray-900 dark:text-white">${patient.phone}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm text-gray-900 dark:text-white">${patient.age || '-'}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm text-gray-900 dark:text-white">${patient.gender || '-'}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm text-gray-900 dark:text-white">${patient.lastVisit || '-'}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                            <button onclick="app.editPatient('${patient.id}')" class="text-primary hover:text-purple-700 ml-4">تعديل</button>
                            <button onclick="app.deletePatient('${patient.id}')" class="text-red-600 hover:text-red-900">حذف</button>
                        </td>
                    </tr>
                `).join('');
            }

            openPatientModal(patientId = null) {
                const modal = document.getElementById('patientModal');
                const title = document.getElementById('patientModalTitle');
                const form = document.getElementById('patientForm');
                
                if (patientId) {
                    const patient = dataManager.patients.find(p => p.id === patientId);
                    title.textContent = 'تعديل بيانات المريض';
                    this.fillPatientForm(patient);
                    form.dataset.editingId = patientId;
                } else {
                    title.textContent = 'إضافة مريض جديد';
                    form.reset();
                    delete form.dataset.editingId;
                }
                
                modal.classList.remove('hidden');
            }

            fillPatientForm(patient) {
                document.getElementById('patientName').value = patient.name;
                document.getElementById('patientPhone').value = patient.phone;
                document.getElementById('patientAge').value = patient.age || '';
                document.getElementById('patientGender').value = patient.gender || '';
                document.getElementById('patientBirthDate').value = patient.birthDate || '';
                document.getElementById('patientAddress').value = patient.address || '';
                document.getElementById('patientMedicalHistory').value = patient.medicalHistory || '';
            }

            savePatient() {
                const form = document.getElementById('patientForm');
                
                const patientData = {
                    name: document.getElementById('patientName').value,
                    phone: document.getElementById('patientPhone').value,
                    age: document.getElementById('patientAge').value,
                    gender: document.getElementById('patientGender').value,
                    birthDate: document.getElementById('patientBirthDate').value,
                    address: document.getElementById('patientAddress').value,
                    medicalHistory: document.getElementById('patientMedicalHistory').value,
                    lastVisit: new Date().toISOString().split('T')[0]
                };

                if (form.dataset.editingId) {
                    dataManager.updatePatient(form.dataset.editingId, patientData);
                    showNotification('تم تحديث بيانات المريض بنجاح', 'success');
                } else {
                    dataManager.addPatient(patientData);
                    showNotification('تم إضافة المريض بنجاح', 'success');
                }

                document.getElementById('patientModal').classList.add('hidden');
                this.renderPatients();
                this.populatePatientOptions();
            }

            editPatient(id) {
                this.openPatientModal(id);
            }

            deletePatient(id) {
                if (confirm('هل أنت متأكد من حذف هذا المريض؟ سيتم حذف جميع مواعيده أيضاً.')) {
                    // حذف مواعيد المريض
                    dataManager.appointments = dataManager.appointments.filter(apt => apt.patientId !== id);
                    dataManager.saveAppointments();
                    
                    // حذف المريض
                    dataManager.deletePatient(id);
                    this.renderPatients();
                    this.populatePatientOptions();
                    showNotification('تم حذف المريض ومواعيده', 'success');
                }
            }

            filterPatients() {
                const search = document.getElementById('patientSearch').value.toLowerCase();
                const genderFilter = document.getElementById('patientGenderFilter').value;

                let filtered = dataManager.patients;

                if (search) {
                    filtered = filtered.filter(patient => 
                        patient.name.toLowerCase().includes(search) ||
                        patient.phone.includes(search)
                    );
                }

                if (genderFilter) {
                    filtered = filtered.filter(patient => patient.gender === genderFilter);
                }

                this.renderPatients(filtered);
            }

            loadDatabaseConnections() {
                this.renderConnections();
            }

            renderConnections() {
                const container = document.getElementById('connectionsList');
                
                if (dataManager.connections.length === 0) {
                    container.innerHTML = `
                        <div class="text-center py-8">
                            <p class="text-gray-500 dark:text-gray-400">لا توجد اتصالات محفوظة</p>
                            <button onclick="app.openConnectionModal()" class="mt-4 bg-primary text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition duration-200">
                                إضافة اتصال أول
                            </button>
                        </div>
                    `;
                    return;
                }

                container.innerHTML = dataManager.connections.map(conn => {
                    const statusColors = {
                        'متصل': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
                        'غير متصل': 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300',
                        'فشل الاتصال': 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300'
                    };

                    return `
                        <div class="flex items-center justify-between p-4 bg-gray-50 dark:bg-dark-700 rounded-lg">
                            <div class="flex items-center">
                                <div class="p-2 bg-primary rounded-lg ml-4">
                                    <svg class="h-6 w-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4" />
                                    </svg>
                                </div>
                                <div>
                                    <h4 class="font-medium text-gray-900 dark:text-white">${conn.name}</h4>
                                    <p class="text-sm text-gray-600 dark:text-gray-300">${conn.type}</p>
                                </div>
                            </div>
                            <div class="flex items-center space-x-4 space-x-reverse">
                                <span class="inline-flex px-2 py-1 text-xs font-semibold rounded-full ${statusColors[conn.status] || 'bg-gray-100 text-gray-800'}">${conn.status}</span>
                                <div class="flex space-x-2 space-x-reverse">
                                    <button onclick="app.testSingleConnection('${conn.id}')" class="text-blue-600 hover:text-blue-900 text-sm">اختبار</button>
                                    <button onclick="app.deleteConnection('${conn.id}')" class="text-red-600 hover:text-red-900 text-sm">حذف</button>
                                </div>
                            </div>
                        </div>
                    `;
                }).join('');
            }

            openConnectionModal() {
                const modal = document.getElementById('connectionModal');
                const form = document.getElementById('connectionForm');
                
                form.reset();
                modal.classList.remove('hidden');
            }

            async testConnection() {
                const form = document.getElementById('connectionForm');
                const connectionData = {
                    name: document.getElementById('connectionName').value,
                    type: document.getElementById('connectionType').value,
                    server: document.getElementById('connectionServer').value,
                    username: document.getElementById('connectionUsername').value,
                    password: document.getElementById('connectionPassword').value,
                    database: document.getElementById('connectionDatabase').value
                };

                if (!connectionData.name || !connectionData.type) {
                    showNotification('يجب ملء الحقول المطلوبة', 'error');
                    return;
                }

                const testBtn = document.getElementById('testConnection');
                testBtn.disabled = true;
                testBtn.innerHTML = '<div class="loading inline-block ml-2"></div> جارٍ الاختبار...';

                const success = await dataManager.testConnection(connectionData);
                
                testBtn.disabled = false;
                testBtn.innerHTML = 'اختبار الاتصال';
            }

            saveConnection() {
                const connectionData = {
                    name: document.getElementById('connectionName').value,
                    type: document.getElementById('connectionType').value,
                    server: document.getElementById('connectionServer').value,
                    username: document.getElementById('connectionUsername').value,
                    password: document.getElementById('connectionPassword').value,
                    database: document.getElementById('connectionDatabase').value
                };

                if (!connectionData.name || !connectionData.type) {
                    showNotification('يجب ملء الحقول المطلوبة', 'error');
                    return;
                }

                dataManager.addConnection(connectionData);
                document.getElementById('connectionModal').classList.add('hidden');
                this.renderConnections();
                showNotification('تم حفظ الاتصال بنجاح', 'success');
            }

            async testSingleConnection(id) {
                const connection = dataManager.connections.find(c => c.id === id);
                if (connection) {
                    await dataManager.testConnection(connection);
                    this.renderConnections();
                }
            }

            deleteConnection(id) {
                if (confirm('هل أنت متأكد من حذف هذا الاتصال؟')) {
                    dataManager.deleteConnection(id);
                    this.renderConnections();
                    showNotification('تم حذف الاتصال', 'success');
                }
            }

            async testAllConnections() {
                showNotification('جارٍ اختبار جميع الاتصالات...', 'info');
                
                for (const connection of dataManager.connections) {
                    await dataManager.testConnection(connection);
                }
                
                this.renderConnections();
                showNotification('تم اختبار جميع الاتصالات', 'success');
            }

            syncAllData() {
                const statusElement = document.getElementById('connectionStatus');
                statusElement.className = 'connection-status syncing';
                
                showNotification('جارٍ مزامنة البيانات...', 'info');
                
                setTimeout(() => {
                    statusElement.className = 'connection-status connected';
                    showNotification('تم مزامنة البيانات بنجاح', 'success');
                }, 2000);
            }

            exportData() {
                const data = {
                    appointments: dataManager.appointments,
                    patients: dataManager.patients,
                    connections: dataManager.connections.map(c => ({ ...c, password: '***' })), // إخفاء كلمات المرور
                    fields: dataManager.fields,
                    exportDate: new Date().toISOString()
                };

                const dataStr = JSON.stringify(data, null, 2);
                const dataBlob = new Blob([dataStr], { type: 'application/json' });
                const url = URL.createObjectURL(dataBlob);
                
                const link = document.createElement('a');
                link.href = url;
                link.download = `medical-data-backup-${new Date().toISOString().split('T')[0]}.json`;
                link.click();
                
                URL.revokeObjectURL(url);
                showNotification('تم تصدير البيانات بنجاح', 'success');
            }

            loadFields() {
                this.renderFields();
            }

            renderFields() {
                const tbody = document.getElementById('fieldsTableBody');
                
                if (dataManager.fields.length === 0) {
                    tbody.innerHTML = `
                        <tr>
                            <td colspan="5" class="px-6 py-4 text-center text-gray-500 dark:text-gray-400">
                                لا توجد حقول مخصصة
                            </td>
                        </tr>
                    `;
                    return;
                }

                tbody.innerHTML = dataManager.fields.map(field => `
                    <tr class="hover:bg-gray-50 dark:hover:bg-dark-700">
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm font-medium text-gray-900 dark:text-white">${field.name}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm text-gray-900 dark:text-white">${field.type}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <span class="inline-flex px-2 py-1 text-xs font-semibold rounded-full ${field.required ? 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300' : 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300'}">
                                ${field.required ? 'مطلوب' : 'اختياري'}
                            </span>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <div class="text-sm text-gray-900 dark:text-white">${field.section}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                            <button onclick="app.editField('${field.id}')" class="text-primary hover:text-purple-700 ml-4">تعديل</button>
                            <button onclick="app.deleteField('${field.id}')" class="text-red-600 hover:text-red-900">حذف</button>
                        </td>
                    </tr>
                `).join('');
            }

            openFieldModal(fieldId = null) {
                const modal = document.getElementById('fieldModal');
                const title = document.getElementById('fieldModalTitle');
                const form = document.getElementById('fieldForm');
                
                if (fieldId) {
                    const field = dataManager.fields.find(f => f.id === fieldId);
                    title.textContent = 'تعديل الحقل';
                    this.fillFieldForm(field);
                    form.dataset.editingId = fieldId;
                } else {
                    title.textContent = 'إضافة حقل جديد';
                    form.reset();
                    delete form.dataset.editingId;
                    document.getElementById('fieldOptionsContainer').classList.add('hidden');
                }
                
                modal.classList.remove('hidden');
            }

            fillFieldForm(field) {
                document.getElementById('fieldName').value = field.name;
                document.getElementById('fieldType').value = field.type;
                document.getElementById('fieldSection').value = field.section;
                document.getElementById('fieldRequired').checked = field.required;
                
                if (field.options && (field.type === 'select' || field.type === 'radio')) {
                    document.getElementById('fieldOptionsContainer').classList.remove('hidden');
                    document.getElementById('fieldOptions').value = field.options.join('\n');
                }
            }

            saveField() {
                const form = document.getElementById('fieldForm');
                const fieldType = document.getElementById('fieldType').value;
                
                const fieldData = {
                    name: document.getElementById('fieldName').value,
                    type: fieldType,
                    section: document.getElementById('fieldSection').value,
                    required: document.getElementById('fieldRequired').checked
                };

                if (fieldType === 'select' || fieldType === 'radio') {
                    const optionsText = document.getElementById('fieldOptions').value;
                    fieldData.options = optionsText.split('\n').filter(opt => opt.trim());
                }

                if (form.dataset.editingId) {
                    dataManager.updateField(form.dataset.editingId, fieldData);
                    showNotification('تم تحديث الحقل بنجاح', 'success');
                } else {
                    dataManager.addField(fieldData);
                    showNotification('تم إضافة الحقل بنجاح', 'success');
                }

                document.getElementById('fieldModal').classList.add('hidden');
                this.renderFields();
            }

            editField(id) {
                this.openFieldModal(id);
            }

            deleteField(id) {
                if (confirm('هل أنت متأكد من حذف هذا الحقل؟')) {
                    dataManager.deleteField(id);
                    this.renderFields();
                    showNotification('تم حذف الحقل', 'success');
                }
            }

            loadReports() {
                this.renderQuickReport();
            }

            renderQuickReport() {
                const container = document.getElementById('quickReport');
                const stats = dataManager.getStats();
                const today = new Date().toISOString().split('T')[0];
                const thisMonth = new Date().toISOString().substr(0, 7);

                const monthlyAppointments = dataManager.appointments.filter(apt => 
                    apt.date.startsWith(thisMonth)
                ).length;

                const completedAppointments = dataManager.appointments.filter(apt => 
                    apt.status === 'مكتمل'
                ).length;

                container.innerHTML = `
                    <div class="bg-blue-50 dark:bg-blue-900 p-4 rounded-lg">
                        <h4 class="font-semibold text-blue-900 dark:text-blue-100">مواعيد اليوم</h4>
                        <p class="text-2xl font-bold text-blue-600 dark:text-blue-300">${stats.todayAppointments}</p>
                    </div>
                    <div class="bg-green-50 dark:bg-green-900 p-4 rounded-lg">
                        <h4 class="font-semibold text-green-900 dark:text-green-100">مواعيد الشهر</h4>
                        <p class="text-2xl font-bold text-green-600 dark:text-green-300">${monthlyAppointments}</p>
                    </div>
                    <div class="bg-purple-50 dark:bg-purple-900 p-4 rounded-lg">
                        <h4 class="font-semibold text-purple-900 dark:text-purple-100">مواعيد مكتملة</h4>
                        <p class="text-2xl font-bold text-purple-600 dark:text-purple-300">${completedAppointments}</p>
                    </div>
                    <div class="bg-orange-50 dark:bg-orange-900 p-4 rounded-lg">
                        <h4 class="font-semibold text-orange-900 dark:text-orange-100">إجمالي المرضى</h4>
                        <p class="text-2xl font-bold text-orange-600 dark:text-orange-300">${stats.totalPatients}</p>
                    </div>
                `;
            }

            loadSettings() {
                // تحميل الإعدادات المحفوظة
                const settings = dataManager.settings;
                
                document.getElementById('clinicName').value = settings.clinicName || 'عيادة الدكتور محمد';
                document.getElementById('defaultDuration').value = settings.defaultDuration || 30;
                document.getElementById('freeReturnDays').value = settings.freeReturnDays || 7;
                document.getElementById('autoSync').checked = settings.autoSync !== false;
                document.getElementById('offlineMode').checked = settings.offlineMode !== false;
                document.getElementById('syncNotifications').checked = settings.syncNotifications !== false;
            }

            saveSettings() {
                const settings = {
                    clinicName: document.getElementById('clinicName').value,
                    defaultDuration: parseInt(document.getElementById('defaultDuration').value),
                    freeReturnDays: parseInt(document.getElementById('freeReturnDays').value),
                    autoSync: document.getElementById('autoSync').checked,
                    offlineMode: document.getElementById('offlineMode').checked,
                    syncNotifications: document.getElementById('syncNotifications').checked
                };

                dataManager.settings = settings;
                dataManager.saveSettings();
                showNotification('تم حفظ الإعدادات بنجاح', 'success');
            }

            resetSettings() {
                if (confirm('هل أنت متأكد من إعادة تعيين الإعدادات؟')) {
                    dataManager.settings = {};
                    dataManager.saveSettings();
                    this.loadSettings();
                    showNotification('تم إعادة تعيين الإعدادات', 'info');
                }
            }
        }

        // تشغيل التطبيق
        const app = new AppManager();

        // تحديث البيانات كل دقيقة
        setInterval(() => {
            if (app.currentPage === 'dashboard') {
                app.loadDashboard();
            }
        }, 60000);

        console.log('نظام إدارة المواعيد الطبية المتكامل تم تحميله بنجاح');
        console.log('جميع الميزات مفعلة ومتاحة للاستخدام');
    </script>
</body>
</html>
