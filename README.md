# Smart_placementSystem
An intelligent, full-stack application leveraging Machine Learning (Scikit-learn/TensorFlow) to optimize campus recruitment. It features a predictive recommendation engine that matches students to job roles based on skills, academics, and historical placement data, significantly boosting efficiency and match quality for students and recruiters.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Placement Dashboard</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Inter font -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Load Heroicons / Ionicons for icons -->
    <script type="module" src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.esm.js"></script>
    <script nomodule src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.js"></script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6; /* gray-100 */
        }
        /* Custom scrollbar for webkit browsers */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #c5c5c5;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #a8a8a8;
        }
        .table-responsive {
            overflow-x: auto;
        }

        /* Subtle hover animations to keep look professional but unchanged */
        .card-hover:hover { transform: translateY(-4px); box-shadow: 0 12px 30px rgba(2,6,23,0.08); }
        .btn-link { transition: all 160ms ease; }
        .btn-link:hover { transform: translateY(-2px); }
    </style>
</head>
<body class="bg-gray-100">

    <div id="app" class="min-h-screen">
        
        <!-- Header -->
        <header class="bg-white shadow-md">
            <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center h-16">
                    <!-- Logo/Title -->
                    <div class="flex-shrink-0 flex items-center">
                        <ion-icon name="school-outline" class="text-3xl text-indigo-600"></ion-icon>
                        <h1 class="text-xl font-bold text-gray-800 ml-2">Smart Placement System</h1>
                    </div>
                    
                    <!-- Toggle Button -->
                    <div class="flex items-center">
                        <span id="currentUser" class="text-sm text-gray-600 mr-4 hidden md:block">Viewing as: <span class="font-semibold text-indigo-700">TPO / Admin</span></span>
                        <button id="toggleRoleBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-lg text-sm font-medium hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 transition-all">
                            Switch to Student View
                        </button>
                    </div>
                </div>
            </nav>
        </header>

        <!-- Main Content Area -->
        <main class="container mx-auto p-4 sm:p-6 lg:p-8">

            <!-- =================================================================== -->
            <!-- =================== TPO / ADMIN DASHBOARD ======================= -->
            <!-- =================================================================== -->
            <div id="adminDashboard">
                <h2 class="text-2xl font-bold text-gray-900 mb-6">TPO / Admin Dashboard</h2>
                
                <!-- KPI Stats Cards -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
                    <!-- Card 1: Total Students -->
                    <div class="bg-white p-6 rounded-xl shadow-lg flex items-center justify-between card-hover">
                        <div>
                            <div class="text-sm font-medium text-gray-500">
                                Total Students
                            </div>
                            <div class="text-3xl font-bold text-gray-900">1200</div>
                            <div class="text-sm text-green-600">950 Eligible</div>
                        </div>
                        <div class="bg-indigo-100 text-indigo-600 p-3 rounded-full">
                            <ion-icon name="people-outline" class="text-2xl"></ion-icon>
                        </div>
                    </div>
                    <!-- Card 2: Active Drives -->
                    <div class="bg-white p-6 rounded-xl shadow-lg flex items-center justify-between card-hover">
                        <div>
                            <div class="text-sm font-medium text-gray-500">Active Drives</div>
                            <div class="text-3xl font-bold text-gray-900">12</div>
                            <div class="text-sm text-blue-600">3 New this week</div>
                        </div>
                        <div class="bg-blue-100 text-blue-600 p-3 rounded-full">
                            <ion-icon name="briefcase-outline" class="text-2xl"></ion-icon>
                        </div>
                    </div>
                    <!-- Card 3: Applications -->
                    <div class="bg-white p-6 rounded-xl shadow-lg flex items-center justify-between card-hover">
                        <div>
                            <div class="text-sm font-medium text-gray-500">Total Applications</div>
                            <div class="text-3xl font-bold text-gray-900">780</div>
                            <div class="text-sm text-yellow-600">45 Pending Review</div>
                        </div>
                        <div class="bg-yellow-100 text-yellow-600 p-3 rounded-full">
                            <ion-icon name="document-text-outline" class="text-2xl"></ion-icon>
                        </div>
                    </div>
                    <!-- Card 4: Placed Students -->
                    <div class="bg-white p-6 rounded-xl shadow-lg flex items-center justify-between card-hover">
                        <div>
                            <div class="text-sm font-medium text-gray-500">Placed Students</div>
                            <div class="text-3xl font-bold text-gray-900">150</div>
                            <div class="text-sm text-gray-500">Target: 400</div>
                        </div>
                        <div class="bg-green-100 text-green-600 p-3 rounded-full">
                            <ion-icon name="ribbon-outline" class="text-2xl"></ion-icon>
                        </div>
                    </div>
                </div>

                <!-- Main Content Grid -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    
                    <!-- Column 1: Quick Actions & Drive Management -->
                    <div class="lg:col-span-2 space-y-6">
                        <!-- Quick Actions -->
                        <div class="bg-white p-6 rounded-xl shadow-lg card-hover">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">Quick Actions</h3>
                            <div class="flex flex-wrap gap-4">
                                <!-- NOTE: create_drive url must exist in Django urls.py -->
                                <a href="Admin_view_files/create_drive.html"  class="flex items-center bg-indigo-600 text-white px-5 py-3 rounded-lg font-medium hover:bg-indigo-700 transition-all btn-link">
                                    <ion-icon name="add-circle-outline" class="text-xl mr-2"></ion-icon>
                                    Create New Drive
                                </a>
                                <a href="Admin_view_files/filterStud.html" target="blank" class="flex items-center bg-gray-700 text-white px-5 py-3 rounded-lg font-medium hover:bg-gray-800 transition-all">
                                    <ion-icon name="filter-outline" class="text-xl mr-2"></ion-icon>
                                    Filter Students
                                </a>
                                
                           <a href="Admin_view_files/send_noti.html" target="blank"
   class="flex items-center bg-blue-500 text-white px-5 py-3 rounded-lg font-medium hover:bg-blue-600 transition-all">
   <ion-icon name="notifications-outline" class="text-xl mr-2"></ion-icon>
   Send Notification
</a>

                                <a href="Admin_view_files/generate_reports.html" target="blank" class="flex items-center bg-gray-200 text-gray-800 px-5 py-3 rounded-lg font-medium hover:bg-gray-300 transition-all">
                                    <ion-icon name="download-outline" class="text-xl mr-2"></ion-icon>
                                    Generate Reports
                                </a>
                            </div>
                        </div>

                        <!-- Active Drives Management -->
                        <div class="bg-white rounded-xl shadow-lg overflow-hidden card-hover">
                            <div class="p-6 border-b border-gray-200">
                                <h3 class="text-lg font-semibold text-gray-900">Active Drives Management</h3>
                            </div>
                            <div class="table-responsive">
                                <table class="min-w-full divide-y divide-gray-200">
                                    <thead class="bg-gray-50">
                                        <tr>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-bold text-gray-600 uppercase tracking-wider">Company</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-bold text-gray-600 uppercase tracking-wider">Role</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-bold text-gray-600 uppercase tracking-wider">Applications</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-bold text-gray-600 uppercase tracking-wider">Deadline</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-bold text-gray-600 uppercase tracking-wider">Actions</th>
                                        </tr>
                                    </thead>
                                    <tbody class="bg-white divide-y divide-gray-200">
                                        <tr>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="flex items-center">
                                                    <img class="h-8 w-8 rounded-full object-contain" src="https://placehold.co/40x40/9ca3af/FFFFFF?text=T" alt="TechCorp Logo">
                                                    <div class="ml-3">
                                                        <div class="text-sm font-medium text-gray-900">TechCorp</div>
                                                        <div class="text-xs text-gray-500">On-Campus</div>
                                                    </div>
                                                </div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="text-sm text-gray-900">Graduate Engineer</div>
                                                <div class="text-xs text-gray-500">CGPA > 7.0</div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <span class="px-3 py-1 inline-flex text-sm leading-5 font-semibold rounded-full bg-blue-100 text-blue-800">150</span>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap text-sm text-red-600 font-medium">Oct 31, 2025</td>
                                            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium space-x-3">
<a href="Admin_view_files/view_edit.html" class="text-indigo-600 hover:text-indigo-900">View</a>
<a href="Admin_view_files/view_edit.html" class="text-gray-500 hover:text-gray-800">Edit</a>

    <a href="#" class="text-red-600 hover:text-red-900">Close</a>
</td>

                                        </tr>
                                        <tr>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="flex items-center">
                                                    <img class="h-8 w-8 rounded-full object-contain" src="https://placehold.co/40x40/6366f1/FFFFFF?text=D" alt="DataFin Logo">
                                                    <div class="ml-3">
                                                        <div class="text-sm font-medium text-gray-900">DataFin</div>
                                                        <div class="text-xs text-gray-500">Internship</div>
                                                    </div>
                                                </div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="text-sm text-gray-900">Data Analyst Intern</div>
                                                <div class="text-xs text-gray-500">CGPA > 8.0</div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <span class="px-3 py-1 inline-flex text-sm leading-5 font-semibold rounded-full bg-blue-100 text-blue-800">95</span>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">Nov 02, 2025</td>
                                            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium space-x-2">
                                                <a href="{% url 'dashboard' %}" class="text-indigo-600 hover:text-indigo-900">View</a>
                                                <a href="#" class="text-gray-500 hover:text-gray-800">Edit</a>
                                                <a href="#" class="text-red-600 hover:text-red-900">Close</a>
                                            </td>
                                        </tr>
                                        <!-- More rows... -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Column 2: Activity Feed & Pending Tasks -->
                    <div class="lg:col-span-1 space-y-6">
                        <!-- Pending Tasks -->
                        <div class="bg-white p-6 rounded-xl shadow-lg card-hover">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">Pending Tasks</h3>
                            <ul class="space-y-4">
                                <li class="flex items-center justify-between">
                                    <div class="flex items-center">
                                        <ion-icon name="alert-circle-outline" class="text-xl text-yellow-500 mr-3"></ion-icon>
                                        <span class="text-sm text-gray-700">5 student profiles incomplete</span>
                                    </div>
                                    <a href="#" class="text-sm text-indigo-600 hover:underline">View List</a>
                                </li>
                                <li class="flex items-center justify-between">
                                    <div class="flex items-center">
                                        <ion-icon name="document-attach-outline" class="text-xl text-red-500 mr-3"></ion-icon>
                                        <span class="text-sm text-gray-700">Resume for 'R. Sharma' needs verification</span>
                                    </div>
                                    <a href="#" class="text-sm text-indigo-600 hover:underline">Verify</a>
                                </li>
                                <li class="flex items-center justify-between">
                                    <div class="flex items-center">
                                        <ion-icon name="checkmark-circle-outline" class="text-xl text-green-500 mr-3"></ion-icon>
                                        <span class="text-sm text-gray-700">'CoreLogic' drive needs approval</span>
                                    </div>
                                    <a href="#" class="text-sm text-indigo-600 hover:underline">Approve</a>
                                </li>
                            </ul>
                        </div>
                        
                        <!-- Recent Activity Feed -->
                        <div class="bg-white p-6 rounded-xl shadow-lg card-hover">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">Recent Activity</h3>
                            <ul class="space-y-4 max-h-96 overflow-y-auto">
                                <li class="flex items-start">
                                    <div class="flex-shrink-0 bg-blue-100 text-blue-600 p-2 rounded-full">
                                        <ion-icon name="person-add-outline" class="text-lg"></ion-icon>
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm text-gray-700"><span class="font-medium text-gray-900">Priya K. (CS)</span> just applied to <span class="font-medium text-gray-900">TechCorp</span></p>
                                        <time class="text-xs text-gray-500">2 mins ago</time>
                                    </div>
                                </li>
                                <li class="flex items-start">
                                    <div class="flex-shrink-0 bg-green-100 text-green-600 p-2 rounded-full">
                                        <ion-icon name="cloud-upload-outline" class="text-lg"></ion-icon>
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm text-gray-700"><span class="font-medium text-gray-900">Aman S. (Mech)</span> just uploaded a new resume.</p>
                                        <time class="text-xs text-gray-500">15 mins ago</time>
                                    </div>
                                </li>
                                <li class="flex items-start">
                                    <div class="flex-shrink-0 bg-indigo-100 text-indigo-600 p-2 rounded-full">
                                        <ion-icon name="business-outline" class="text-lg"></ion-icon>
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm text-gray-700"><span class="font-medium text-gray-900">New Company 'CoreLogic'</span> registered.</p>
                                        <time class="text-xs text-gray-500">1 hour ago</time>
                                    </div>
                                </li>
                                <!-- More activity... -->
                            </ul>
                        </div>
                    </div>
                </div>
            </div>

            <!-- =================================================================== -->

            <!-- ======================= STUDENT DASHBOARD ======================= -->
            <!-- =================================================================== -->
            <div id="studentDashboard" class="hidden">
                <h2 class="text-2xl font-bold text-gray-900 mb-6">Student Dashboard</h2>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    
                    <!-- Column 1: Profile Summary -->
                    <div class="lg:col-span-1 space-y-6">
                        <div class="bg-white p-6 rounded-xl shadow-lg text-center card-hover">
                            <img src="https://placehold.co/100x100/e0e7ff/4f46e5?text=AV" alt="Avatar" class="w-24 h-24 rounded-full mx-auto mb-4 border-4 border-indigo-200">
                            <h3 class="text-lg font-semibold text-gray-900">Adwait Rajendra Dhaygude</h3>
                            <p class="text-sm text-gray-600">B.Tech - Electrical & Computer Science</p>
                            <p class="text-sm text-gray-800 font-medium mt-1">CGPA: <span class="text-indigo-600">8.4</span></p>
                            
                            <div class="mt-4">
                                <p class="text-sm text-gray-600">Profile Completion: 85%</p>
                                <div class="w-full bg-gray-200 rounded-full h-2.5 mt-1">
                                    <div class="bg-indigo-600 h-2.5 rounded-full" style="width: 85%"></div>
                                </div>
                                <p class="text-xs text-red-600 mt-1">Missing 'Project Details'</p>
                            </div>

                            <div class="mt-6 space-y-3">
                                <a href="Student_view_files/upload_resume.html" target="blank" class="w-full inline-flex items-center justify-center bg-indigo-600 text-white px-4 py-3 rounded-lg font-medium hover:bg-indigo-700 transition-all">
                                    <ion-icon name="cloud-upload-outline" class="text-xl mr-2"></ion-icon>
                                    Upload New Resume
                                </a>
                                <a href="Student_view_files/edit_profile.html" target="blank" class="w-full inline-flex items-center justify-center bg-gray-200 text-gray-800 px-4 py-3 rounded-lg font-medium hover:bg-gray-300 transition-all">
                                    <ion-icon name="create-outline" class="text-xl mr-2"></ion-icon>
                                    Edit Full Profile
                                </a>
                            </div>
                        </div>

                        <!-- Quick Stats -->
                        <div class="bg-white p-6 rounded-xl shadow-lg card-hover">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">My Application Stats</h3>
                            <ul class="space-y-3">
                                <li class="flex justify-between items-center text-sm">
                                    <span class="text-gray-600">Active Drives on Campus</span>
                                    <span class="font-bold text-gray-900 text-base">12</span>
                                </li>
                                <li class="flex justify-between items-center text-sm">
                                    <span class="text-gray-600">Drives I'm Eligible For</span>
                                    <span class="font-bold text-green-600 text-base">5</span>
                                </li>
                                <li class="flex justify-between items-center text-sm">
                                    <span class="text-gray-600">Applications Submitted</span>
                                    <span class="font-bold text-blue-600 text-base">3</span>
                                </li>
                                <li class="flex justify-between items-center text-sm">
                                    <span class="text-gray-600">Shortlisted / Interviews</span>
                                    <span class="font-bold text-indigo-600 text-base">1</span>
                                </li>
                            </ul>
                        </div>
                    </div>
                    
                    <!-- Column 2: Main Feed (Notifications & Drives) -->
                    <div class="lg:col-span-2 space-y-6">
                        
                        <!-- Notifications -->
                        <div class="bg-white p-6 rounded-xl shadow-lg card-hover">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">🔔 Notifications</h3>
                            <ul class="space-y-4 max-h-60 overflow-y-auto">
                                <li class="flex items-start p-4 bg-green-50 border border-green-200 rounded-lg">
                                    <div class="flex-shrink-0 bg-green-100 text-green-600 p-2 rounded-full">
                                        <ion-icon name="checkmark-circle-outline" class="text-lg"></ion-icon>
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm text-green-800"><span class="font-medium">Shortlisted!</span> Your application for <span class="font-medium">TechCorp</span> has been shortlisted for the technical round.</p>
                                        <time class="text-xs text-green-600">10 mins ago</time>
                                    </div>
                                </li>
                                <li class="flex items-start p-4 bg-yellow-50 border border-yellow-200 rounded-lg">
                                    <div class="flex-shrink-0 bg-yellow-100 text-yellow-600 p-2 rounded-full">
                                        <ion-icon name="alarm-outline" class="text-lg"></ion-icon>
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm text-yellow-800"><span class="font-medium">Deadline Tomorrow:</span> The application for <span class="font-medium">DataFin</span> closes tomorrow.</p>
                                        <time class="text-xs text-yellow-600">1 hour ago</time>
                                    </div>
                                </li>
                                <li class="flex items-start p-4 bg-blue-50 border border-blue-200 rounded-lg">
                                    <div class="flex-shrink-0 bg-blue-100 text-blue-600 p-2 rounded-full">
                                        <ion-icon name="briefcase-outline" class="text-lg"></ion-icon>
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm text-blue-800"><span class="font-medium">New Drive:</span> <span class="font-medium">CoreLogic</span> is hiring. You are eligible.</p>
                                        <time class="text-xs text-blue-600">3 hours ago</time>
                                    </div>
                                </li>
                            </ul>
                        </div>
                        
                        <!-- Eligible Drives -->
                        <div class="space-y-4">
                            <h3 class="text-lg font-semibold text-gray-900">🚀 My Eligible Drives</h3>

                            <!-- Drive Card 1 (Eligible) -->
                            <div class="bg-white rounded-xl shadow-lg overflow-hidden transition-all hover:shadow-xl card-hover">
                                <div class="p-6">
                                    <div class="flex justify-between items-start">
                                        <div>
                                            <div class="flex items-center mb-1">
                                                <img class="h-8 w-8 rounded-full object-contain mr-3" src="https://placehold.co/40x40/9ca3af/FFFFFF?text=T" alt="TechCorp Logo">
                                                <h4 class="text-lg font-semibold text-gray-900">TechCorp</h4>
                                            </div>
                                            <p class="text-base font-medium text-indigo-600">Graduate Engineer Trainee</p>
                                        </div>
                                        <span class="px-3 py-1 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">You are Eligible</span>
                                    </div>
                                    <div class="mt-4 border-t border-gray-200 pt-4">
                                        <div class="flex justify-between items-center text-sm">
                                            <p class="text-gray-600">Eligibility: <span class="font-medium text-gray-800">CGPA > 7.0, CS/IT</span></p>
                                            <p class="text-red-600 font-medium">Deadline: Oct 31</p>
                                        </div>
                                        <!-- example placeholder url: replace with actual drive id-based URL later -->
                                        <a href="Student_view_files/drive_view.html" class="mt-4 block w-full bg-indigo-600 text-white px-4 py-2 rounded-lg font-medium hover:bg-indigo-700 transition-all text-center">
                                            View Details & Apply
                                        </a>
                                    </div>
                                </div>
                            </div>

                            <!-- Drive Card 2 (Ineligible) -->
                            <div class="bg-white rounded-xl shadow-lg overflow-hidden transition-all hover:shadow-xl opacity-70 card-hover">
                                <div class="p-6">
                                    <div class="flex justify-between items-start">
                                        <div>
                                            <div class="flex items-center mb-1">
                                                <img class="h-8 w-8 rounded-full object-contain mr-3" src="https://placehold.co/40x40/6366f1/FFFFFF?text=D" alt="DataFin Logo">
                                                <h4 class="text-lg font-semibold text-gray-900">DataFin</h4>
                                            </div>
                                            <p class="text-base font-medium text-indigo-600">Data Analyst Intern</p>
                                        </div>
                                        <span class="px-3 py-1 inline-flex text-xs leading-5 font-semibold rounded-full bg-red-100 text-red-800">Not Eligible (CGPA &lt; 8.0)</span>
                                    </div>
                                    <div class="mt-4 border-t border-gray-200 pt-4">
                                        <div class="flex justify-between items-center text-sm">
                                            <p class="text-gray-600">Eligibility: <span class="font-medium text-gray-800">CGPA &gt; 8.0, All Branches</span></p>
                                            <p class="text-gray-600 font-medium">Deadline: Nov 02</p>
                                        </div>
                                        <button class="mt-4 w-full bg-gray-300 text-gray-600 px-4 py-2 rounded-lg font-medium cursor-not-allowed">
                                            View Details
                                        </button>
                                    </div>
                                </div>
                            </div>
                            
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

<script>
document.addEventListener('DOMContentLoaded', () => {
    const toggleRoleBtn = document.getElementById('toggleRoleBtn');
    const adminDashboard = document.getElementById('adminDashboard');
    const studentDashboard = document.getElementById('studentDashboard');
    const currentUser = document.getElementById('currentUser');
    const currentUserSpan = currentUser.querySelector('span');

    // Load saved role from localStorage (default to 'admin' if not set)
    let currentRole = localStorage.getItem('currentRole') || 'admin';

    // Function to update the dashboard view based on role
    function updateView() {
        if (currentRole === 'admin') {
            adminDashboard.classList.remove('hidden');
            studentDashboard.classList.add('hidden');
            toggleRoleBtn.textContent = 'Switch to Student View';
            currentUserSpan.textContent = 'TPO / Admin';
            currentUser.classList.add('md:block');
        } else {
            adminDashboard.classList.add('hidden');
            studentDashboard.classList.remove('hidden');
            toggleRoleBtn.textContent = 'Switch to Admin View';
            currentUserSpan.textContent = 'Student';
            currentUser.classList.remove('md:block');
        }
    }

    // Initialize view on page load
    updateView();

    // Toggle role on button click
    toggleRoleBtn.addEventListener('click', () => {
        currentRole = (currentRole === 'admin') ? 'student' : 'admin';
        localStorage.setItem('currentRole', currentRole); // save role
        updateView();
    });
});
</script>

</body>
</html>
