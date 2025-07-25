<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Urology Patient Follow-Up</title>
    
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore-compat.js"></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f5;
            line-height: 1.6;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        .user-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(255,255,255,0.1);
            padding: 10px 20px;
            border-radius: 5px;
            margin-top: 10px;
        }
        
        .btn {
            background: #4CAF50;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
            margin: 5px;
        }
        
        .btn:hover {
            background: #45a049;
        }
        
        .btn-danger {
            background: #f44336;
        }
        
        .btn-danger:hover {
            background: #da190b;
        }
        
        .btn-primary {
            background: #2196F3;
        }
        
        .btn-primary:hover {
            background: #1976D2;
        }
        
        .btn-secondary {
            background: #757575;
        }
        
        .btn-secondary:hover {
            background: #616161;
        }
        
        .login-container {
            max-width: 400px;
            margin: 100px auto;
            background: white;
            padding: 40px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333;
        }
        
        .form-group input, .form-group select, .form-group textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }
        
        .form-group textarea {
            height: 100px;
            resize: vertical;
        }
        
        .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
            border-color: #667eea;
            outline: none;
        }
        
        .form-row {
            display: flex;
            gap: 15px;
        }
        
        .form-row .form-group {
            flex: 1;
        }
        
        .error-message {
            color: #f44336;
            margin-top: 10px;
            padding: 10px;
            background: #ffebee;
            border-radius: 5px;
            display: none;
        }
        
        .success-message {
            color: #4CAF50;
            margin-top: 10px;
            padding: 10px;
            background: #e8f5e8;
            border-radius: 5px;
            display: none;
        }
        
        .hidden {
            display: none !important;
        }
        
        .loading {
            text-align: center;
            padding: 20px;
        }
        
        .spinner {
            border: 4px solid #f3f3f3;
            border-radius: 50%;
            border-top: 4px solid #667eea;
            width: 40px;
            height: 40px;
            animation: spin 2s linear infinite;
            margin: 0 auto;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .patient-card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            border-left: 5px solid #667eea;
        }
        
        .patient-card h3 {
            color: #333;
            margin-bottom: 10px;
        }
        
        .patient-info {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .patient-info span {
            color: #666;
        }
        
        .patient-actions {
            text-align: right;
        }
        
        .search-box {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .search-box input {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }
        
        .form-container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        
        .form-container h2 {
            margin-bottom: 25px;
            color: #333;
            border-bottom: 2px solid #667eea;
            padding-bottom: 10px;
        }
        
        .status-badge {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
            text-transform: uppercase;
        }
        
        .status-pre-op {
            background: #e3f2fd;
            color: #1976d2;
        }
        
        .status-post-op {
            background: #fff3e0;
            color: #f57c00;
        }
        
        .status-discharged {
            background: #e8f5e8;
            color: #388e3c;
        }
        .patient-detail-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .detail-section {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }
        
        .detail-section h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.2rem;
            border-bottom: 1px solid #ddd;
            padding-bottom: 8px;
        }
        
        .detail-section p {
            margin-bottom: 10px;
            line-height: 1.6;
        }
        
        .detail-section strong {
            color: #555;
            display: inline-block;
            min-width: 120px;
        }
        
        .btn-sm {
            padding: 8px 16px;
            font-size: 14px;
        }
        
        .confirmation-dialog {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .confirmation-content {
            background: white;
            padding: 30px;
            border-radius: 10px;
            max-width: 400px;
            text-align: center;
            box-shadow: 0 4px 20px rgba(0,0,0,0.3);
        }
        
        .confirmation-content h3 {
            color: #f44336;
            margin-bottom: 15px;
        }
        
        .confirmation-content p {
            margin-bottom: 25px;
            color: #666;
        }
        
        .patient-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 15px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-radius: 8px;
        }
        
        .patient-header h2 {
            margin: 0;
        }
        
        .patient-meta {
            display: flex;
            gap: 20px;
            font-size: 14px;
        }
        
        @media (max-width: 768px) {
            .patient-detail-grid {
                grid-template-columns: 1fr;
            }
            
            .form-row {
                flex-direction: column;
            }
            
            .patient-header {
                flex-direction: column;
                gap: 10px;
                text-align: center;
            }
            
            .patient-meta {
                justify-content: center;
                flex-wrap: wrap;
            }
        }
        .template-btn {
            padding: 8px 12px;
            font-size: 13px;
            border-radius: 5px;
            border: 2px solid #ddd;
            background: white;
            color: #666;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }
        
        .template-btn:hover {
            border-color: #2196F3;
            background: #e3f2fd;
            color: #1976D2;
            transform: translateY(-1px);
        }
        
        .template-btn.active {
            background: #2196F3;
            color: white;
            border-color: #2196F3;
        }
        
        .operative-note-card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            border-left: 5px solid #4CAF50;
        }
        
        .operative-note-card h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.3rem;
        }
        
        .operative-note-header {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 8px;
        }
        
        .operative-note-header span {
            color: #555;
            font-size: 14px;
        }
        
        .operative-note-header strong {
            color: #333;
            display: inline-block;
            min-width: 80px;
        }
        
        .operative-section {
            margin-bottom: 20px;
        }
        
        .operative-section h4 {
            color: #2196F3;
            margin-bottom: 10px;
            font-size: 1.1rem;
            border-bottom: 2px solid #e3f2fd;
            padding-bottom: 5px;
        }
        
        .operative-section p {
            color: #666;
            line-height: 1.8;
            white-space: pre-wrap;
        }
        
        .template-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 8px;
            max-height: 200px;
            overflow-y: auto;
            padding: 5px;
        }
        
        .template-preview {
            background: #f0f8ff;
            border: 1px solid #e3f2fd;
            border-radius: 5px;
            padding: 15px;
            margin-top: 15px;
            display: none;
        }
        
        .template-preview.show {
            display: block;
        }
        
        .template-preview h4 {
            color: #1976D2;
            margin-bottom: 10px;
            font-size: 1rem;
        }
        
        .template-preview p {
            font-size: 13px;
            color: #666;
            line-height: 1.5;
            margin-bottom: 8px;
        }
        
        .no-operative-notes {
            text-align: center;
            padding: 40px;
            color: #666;
            background: #f9f9f9;
            border-radius: 10px;
            border: 2px dashed #ddd;
        }
        
        .operative-note-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #eee;
            font-size: 13px;
            color: #888;
        }
        
        .operative-note-actions {
            display: flex;
            gap: 10px;
        }

        .daily-log-card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            border-left: 5px solid #FF9800;
        }
        
        .daily-log-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #e3f2fd;
        }
        
        .daily-log-header h3 {
            color: #333;
            margin: 0;
            font-size: 1.2rem;
        }
        
        .daily-log-meta {
            display: flex;
            gap: 15px;
            font-size: 13px;
            color: #666;
        }
        
        .vitals-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 8px;
        }
        
        .vital-item {
            text-align: center;
        }
        
        .vital-label {
            font-size: 11px;
            color: #666;
            text-transform: uppercase;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .vital-value {
            font-size: 16px;
            font-weight: bold;
            color: #333;
        }
        
        .vital-value.abnormal {
            color: #f44336;
        }
        
        .vital-value.normal {
            color: #4CAF50;
        }
        
        .daily-log-content {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
        }
        
        .log-section {
            padding: 10px;
            background: #f9f9f9;
            border-radius: 5px;
            border-left: 3px solid #2196F3;
        }
        
        .log-section h4 {
            color: #1976D2;
            margin: 0 0 8px 0;
            font-size: 14px;
        }
        
        .log-section p {
            margin: 0;
            color: #555;
            line-height: 1.4;
            font-size: 14px;
        }
        
        .daily-log-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #eee;
        }
        
        .no-daily-logs {
            text-align: center;
            padding: 40px;
            color: #666;
            background: #f9f9f9;
            border-radius: 10px;
            border: 2px dashed #ddd;
        }
        
        .chart-container {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .view-toggle {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .view-toggle .btn.active {
            background: #2196F3;
            color: white;
        }
        
        @media (max-width: 768px) {
            .template-grid {
                grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
                gap: 6px;
            }
            
            .template-btn {
                padding: 6px 8px;
                font-size: 12px;
            }
            
            .operative-note-header {
                grid-template-columns: 1fr;
                gap: 10px;
            }
            
            .operative-note-meta {
                flex-direction: column;
                gap: 10px;
                text-align: center;
            }
            
            .operative-note-actions {
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <!-- Loading Screen -->
    <div id="loadingScreen" class="loading">
        <div class="spinner"></div>
        <p>Loading application...</p>
    </div>
    
    <!-- Login Screen -->
    <div id="loginScreen" class="login-container hidden">
        <h2 style="text-align: center; margin-bottom: 30px; color: #333;">
            🏥 Urology Patient System
        </h2>
        <form id="loginForm">
            <div class="form-group">
                <label for="email">Email:</label>
                <input type="email" id="email" required>
            </div>
            <div class="form-group">
                <label for="password">Password:</label>
                <input type="password" id="password" required>
            </div>
            <button type="submit" class="btn btn-primary" style="width: 100%;">
                Login
            </button>
            <div id="loginError" class="error-message"></div>
        </form>
    </div>
    
    <!-- Main Application -->
    <div id="mainApp" class="container hidden">
        <div class="header">
            <h1>🏥 Urology Patient Follow-Up</h1>
            <div class="user-info">
                <div>
                    <span id="userEmail"></span> | 
                    <span id="userRole"></span>
                </div>
                <button id="logoutBtn" class="btn btn-danger">Logout</button>
            </div>
        </div>
        
        <!-- Navigation -->
        <div id="navigation" style="margin-bottom: 20px;">
            <button id="showPatientsBtn" class="btn btn-primary">📋 All Patients</button>
            <button id="showAddPatientBtn" class="btn">➕ Add New Patient</button>
        </div>
        
        <!-- Content Area -->
        <div id="contentArea">
            <!-- Patients List View -->
            <div id="patientsView">
                <div class="search-box">
                    <input type="text" id="searchInput" placeholder="🔍 Search by name, MRN, or room number...">
                </div>
                
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                    <h2>All Patients (<span id="patientCount">0</span>)</h2>
                    <div>
                        <select id="statusFilter" style="padding: 8px; border-radius: 5px; border: 2px solid #ddd;">
                            <option value="">All Status</option>
                            <option value="pre-op">Pre-Op</option>
                            <option value="post-op">Post-Op</option>
                            <option value="discharged">Discharged</option>
                        </select>
                    </div>
                </div>
                
                <div id="patientsList">
                    <div class="loading">
                        <div class="spinner"></div>
                        <p>Loading patients...</p>
                    </div>
                </div>
            </div>
            
            <!-- Add Patient Form -->
            <div id="addPatientView" class="hidden">
                <div class="form-container">
                    <h2>➕ Add New Patient</h2>
                    <form id="addPatientForm">
                        <div class="form-row">
                            <div class="form-group">
                                <label for="patientName">Patient Name *</label>
                                <input type="text" id="patientName" required>
                            </div>
                            <div class="form-group">
                                <label for="patientMRN">MRN *</label>
                                <input type="text" id="patientMRN" required>
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="patientAge">Age</label>
                                <input type="number" id="patientAge" min="0" max="150">
                            </div>
                            <div class="form-group">
                                <label for="patientGender">Gender</label>
                                <select id="patientGender">
                                    <option value="">Select Gender</option>
                                    <option value="Male">Male</option>
                                    <option value="Female">Female</option>
                                    <option value="Other">Other</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="patientPhone">Phone</label>
                                <input type="tel" id="patientPhone">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="roomNumber">Room Number</label>
                                <input type="text" id="roomNumber">
                            </div>
                            <div class="form-group">
                                <label for="admissionDate">Admission Date</label>
                                <input type="date" id="admissionDate">
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="presentingComplaint">Presenting Complaint</label>
                            <textarea id="presentingComplaint" placeholder="Chief complaint and history of present illness..."></textarea>
                        </div>
                        
                        <div class="form-group">
                            <label for="pmh">Past Medical History</label>
                            <textarea id="pmh" placeholder="Relevant past medical history..."></textarea>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="allergies">Allergies</label>
                                <textarea id="allergies" placeholder="Known allergies..."></textarea>
                            </div>
                            <div class="form-group">
                                <label for="medications">Current Medications</label>
                                <textarea id="medications" placeholder="Current medications..."></textarea>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="physicalExam">Physical Examination</label>
                            <textarea id="physicalExam" placeholder="Physical examination findings..."></textarea>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="imagingSummary">Imaging Summary</label>
                                <textarea id="imagingSummary" placeholder="Imaging results and interpretation..."></textarea>
                            </div>
                            <div class="form-group">
                                <label for="labsSummary">Labs Summary</label>
                                <textarea id="labsSummary" placeholder="Laboratory results..."></textarea>
                            </div>
                        </div>
                        
                        <div style="text-align: right; margin-top: 30px;">
                            <button type="button" id="cancelAddPatient" class="btn btn-secondary">Cancel</button>
                            <button type="submit" class="btn btn-primary">Save Patient</button>
                        </div>
                        
                        <div id="addPatientError" class="error-message"></div>
                        <div id="addPatientSuccess" class="success-message"></div>
                    </form>
                </div>
            </div>
            <!-- Patient Detail View -->
            <div id="patientDetailView" class="hidden">
                <div class="form-container">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
                        <h2 id="patientDetailName">Patient Details</h2>
                        <div>
                            <button id="editPatientBtn" class="btn btn-secondary">✏️ Edit Patient</button>
                            <button id="backToPatientsBtn" class="btn btn-primary">← Back to List</button>
                        </div>
                    </div>
                    
                    <div id="patientDetailContent">
                        <!-- Patient info will be loaded here -->
                    </div>
                    
                     <!-- Patient Actions -->
                    <div style="margin-top: 30px; text-align: center;">
                        <button id="addOperativeNoteBtn" class="btn btn-primary">🔪 Add Operative Note</button>
                        <button id="viewDailyLogsBtn" class="btn btn-primary" style="margin-left: 10px;">📊 Daily Logs</button>
                        <button id="createDischargeBtn" class="btn" style="margin-left: 10px;">🏠 Discharge Summary</button>
                        <button id="deletePatientBtn" class="btn btn-danger" style="margin-left: 20px;">🗑️ Delete Patient</button>
                    </div>
                </div>
            </div>
            
            <!-- Edit Patient View -->
            <div id="editPatientView" class="hidden">
                <div class="form-container">
                    <h2>✏️ Edit Patient</h2>
                    <form id="editPatientForm">
                        <div class="form-row">
                            <div class="form-group">
                                <label for="editPatientName">Patient Name *</label>
                                <input type="text" id="editPatientName" required>
                            </div>
                            <div class="form-group">
                                <label for="editPatientMRN">MRN *</label>
                                <input type="text" id="editPatientMRN" required>
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="editPatientAge">Age</label>
                                <input type="number" id="editPatientAge" min="0" max="150">
                            </div>
                            <div class="form-group">
                                <label for="editPatientGender">Gender</label>
                                <select id="editPatientGender">
                                    <option value="">Select Gender</option>
                                    <option value="Male">Male</option>
                                    <option value="Female">Female</option>
                                    <option value="Other">Other</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="editPatientPhone">Phone</label>
                                <input type="tel" id="editPatientPhone">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="editRoomNumber">Room Number</label>
                                <input type="text" id="editRoomNumber">
                            </div>
                            <div class="form-group">
                                <label for="editAdmissionDate">Admission Date</label>
                                <input type="date" id="editAdmissionDate">
                            </div>
                            <div class="form-group">
                                <label for="editPatientStatus">Status</label>
                                <select id="editPatientStatus">
                                    <option value="pre-op">Pre-Op</option>
                                    <option value="post-op">Post-Op</option>
                                    <option value="discharged">Discharged</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="editPresentingComplaint">Presenting Complaint</label>
                            <textarea id="editPresentingComplaint" placeholder="Chief complaint and history of present illness..."></textarea>
                        </div>
                        
                        <div class="form-group">
                            <label for="editPmh">Past Medical History</label>
                            <textarea id="editPmh" placeholder="Relevant past medical history..."></textarea>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="editAllergies">Allergies</label>
                                <textarea id="editAllergies" placeholder="Known allergies..."></textarea>
                            </div>
                            <div class="form-group">
                                <label for="editMedications">Current Medications</label>
                                <textarea id="editMedications" placeholder="Current medications..."></textarea>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="editPhysicalExam">Physical Examination</label>
                            <textarea id="editPhysicalExam" placeholder="Physical examination findings..."></textarea>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="editImagingSummary">Imaging Summary</label>
                                <textarea id="editImagingSummary" placeholder="Imaging results and interpretation..."></textarea>
                            </div>
                            <div class="form-group">
                                <label for="editLabsSummary">Labs Summary</label>
                                <textarea id="editLabsSummary" placeholder="Laboratory results..."></textarea>
                            </div>
                        </div>
                        
                        <div style="text-align: right; margin-top: 30px;">
                            <button type="button" id="cancelEditPatient" class="btn btn-secondary">Cancel</button>
                            <button type="submit" class="btn btn-primary">Update Patient</button>
                        </div>
                        
                        <div id="editPatientError" class="error-message"></div>
                        <div id="editPatientSuccess" class="success-message"></div>
                    </form>
                </div>
            </div>
        </div>
        <!-- Add Operative Note View -->
            <div id="operativeNoteView" class="hidden">
                <div class="form-container">
                    <h2>🔪 Add Operative Note</h2>
                    
                    <!-- Surgery Templates -->
                    <div style="margin-bottom: 25px; padding: 15px; background: #f0f8ff; border-radius: 8px; border-left: 4px solid #2196F3;">
                        <h3 style="margin-bottom: 15px; color: #333;">📋 Surgery Templates (Click to Auto-Fill)</h3>
                        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 10px;">
                            <button type="button" class="btn btn-secondary template-btn" data-template="urs_laser">🔹 URS + Laser</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="flexible_urs">🔹 Flexible URS</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="turp">🔹 TURP</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="orchiopexy">🔹 Orchiopexy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="inguinal_hernia">🔹 Inguinal Hernia</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="pediatric_hernia">🔹 Pediatric Hernia</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="hydrocelectomy">🔹 Hydrocelectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="varicocelectomy">🔹 Varicocelectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="urethrotomy">🔹 Urethrotomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="circumcision">🔹 Circumcision</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="pcnl">🔹 PCNL</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="turbt">🔹 TURBT</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="radical_nephrectomy">🔹 Radical Nephrectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="partial_nephrectomy">🔹 Partial Nephrectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="radical_prostatectomy">🔹 Radical Prostatectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="ureteric_reimplant">🔹 Ureteric Reimplant</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="pyeloplasty">🔹 Pyeloplasty</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="cystolitholapaxy">🔹 Cystolitholapaxy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="radical_orchiectomy">🔹 Radical Orchiectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="simple_orchiectomy">🔹 Simple Orchiectomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="scrotal_exploration">🔹 Scrotal Exploration</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="suprapubic_cystostomy">🔹 Suprapubic Cystostomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="penile_prosthesis">🔹 Penile Prosthesis</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="bladder_neck_incision">🔹 Bladder Neck Incision</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="urethroplasty">🔹 Urethroplasty</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="meatotomy">🔹 Meatotomy</button>
                            <button type="button" class="btn btn-secondary template-btn" data-template="hypospadias">🔹 Hypospadias Repair</button>
                            <button type="button" class="btn template-btn" data-template="other" style="background: #ff9800; color: white;">✏️ Other Surgery</button>
                        </div>
                    </div>
                    
                    <form id="operativeNoteForm">
                        <div class="form-row">
                            <div class="form-group">
                                <label for="surgeryType">Surgery Type *</label>
                                <input type="text" id="surgeryType" required>
                            </div>
                            <div class="form-group">
                                <label for="surgeryDate">Surgery Date *</label>
                                <input type="date" id="surgeryDate" required>
                            </div>
                            <div class="form-group">
                                <label for="surgeryTime">Surgery Time</label>
                                <input type="time" id="surgeryTime">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="surgeon">Surgeon *</label>
                                <input type="text" id="surgeon" required>
                            </div>
                            <div class="form-group">
                                <label for="assistants">Assistant(s)</label>
                                <input type="text" id="assistants">
                            </div>
                            <div class="form-group">
                                <label for="anesthesiaType">Anesthesia Type</label>
                                <select id="anesthesiaType">
                                    <option value="">Select Type</option>
                                    <option value="General anesthesia">General</option>
                                    <option value="Spinal anesthesia">Spinal</option>
                                    <option value="Local anesthesia">Local</option>
                                    <option value="Local + sedation">Local + Sedation</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="position">Position</label>
                            <select id="position">
                                <option value="">Select Position</option>
                                <option value="Lithotomy position">Lithotomy</option>
                                <option value="Supine position">Supine</option>
                                <option value="Prone position">Prone</option>
                                <option value="Lateral decubitus position">Lateral Decubitus</option>
                                <option value="Flank position">Flank</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="findings">Findings</label>
                            <textarea id="findings" placeholder="Operative findings and observations..." style="height: 120px;"></textarea>
                        </div>
                        
                        <div class="form-group">
                            <label for="operativeSteps">Operative Steps</label>
                            <textarea id="operativeSteps" placeholder="Detailed operative procedure steps..." style="height: 200px;"></textarea>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="complications">Complications</label>
                                <textarea id="complications" placeholder="Any complications encountered..."></textarea>
                            </div>
                            <div class="form-group">
                                <label for="bloodLoss">Estimated Blood Loss</label>
                                <input type="text" id="bloodLoss" placeholder="e.g., <50 mL, 100-200 mL">
                            </div>
                        </div>
                        
                        <div style="text-align: right; margin-top: 30px;">
                            <button type="button" id="cancelOperativeNote" class="btn btn-secondary">Cancel</button>
                            <button type="submit" class="btn btn-primary">Save Operative Note</button>
                        </div>
                        
                        <div id="operativeNoteError" class="error-message"></div>
                        <div id="operativeNoteSuccess" class="success-message"></div>
                    </form>
                </div>
            </div>

           <!-- Daily Post-Op Logs View -->
<div id="dailyLogsView" class="hidden">
    <div class="form-container">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
            <h2>📊 Daily Post-Op Logs</h2>
            <div>
                <button id="addDailyLogBtn" class="btn btn-primary">➕ Add Today's Log</button>
                <button id="backFromDailyLogs" class="btn btn-secondary">← Back to Patient</button>
            </div>
        </div>
        
        <!-- Chart Toggle -->
        <div style="text-align: center; margin-bottom: 20px;">
            <button id="showTableView" class="btn btn-primary">📋 Table View</button>
            <button id="showChartView" class="btn btn-secondary">📈 Chart View</button>
        </div>
        
        <!-- Table View -->
        <div id="logsTableView">
            <div id="dailyLogsList">
                <div class="loading">
                    <div class="spinner"></div>
                    <p>Loading daily logs...</p>
                </div>
            </div>
        </div>
        
        <!-- Chart View -->
        <div id="logsChartView" class="hidden">
            <div style="background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
                <h3 style="text-align: center; margin-bottom: 20px;">📈 Vitals Trends</h3>
                <div id="vitalsChart" style="width: 100%; height: 400px;"></div>
            </div>
        </div>
    </div>
</div>

<!-- Add Daily Log Form -->
<div id="addDailyLogView" class="hidden">
    <div class="form-container">
        <h2>➕ Add Daily Log</h2>
        <form id="dailyLogForm">
            <div class="form-row">
                <div class="form-group">
                    <label for="logDate">Date *</label>
                    <input type="date" id="logDate" required>
                </div>
                <div class="form-group">
                    <label for="logTime">Time</label>
                    <input type="time" id="logTime">
                </div>
                <div class="form-group">
                    <label for="dayPostOp">Day Post-Op</label>
                    <input type="number" id="dayPostOp" min="0" placeholder="e.g., 1, 2, 3">
                </div>
            </div>
            
            <!-- Vitals Section -->
            <div style="background: #f0f8ff; padding: 15px; border-radius: 8px; margin-bottom: 20px;">
                <h3 style="color: #1976D2; margin-bottom: 15px;">🩺 Vital Signs</h3>
                <div class="form-row">
                    <div class="form-group">
                        <label for="systolicBP">Systolic BP (mmHg)</label>
                        <input type="number" id="systolicBP" min="60" max="250" placeholder="120">
                    </div>
                    <div class="form-group">
                        <label for="diastolicBP">Diastolic BP (mmHg)</label>
                        <input type="number" id="diastolicBP" min="40" max="150" placeholder="80">
                    </div>
                    <div class="form-group">
                        <label for="heartRate">Heart Rate (bpm)</label>
                        <input type="number" id="heartRate" min="40" max="200" placeholder="72">
                    </div>
                </div>
                
                <div class="form-row">
                    <div class="form-group">
                        <label for="temperature">Temperature (°C)</label>
                        <input type="number" id="temperature" min="35" max="42" step="0.1" placeholder="36.5">
                    </div>
                    <div class="form-group">
                        <label for="respiratoryRate">Respiratory Rate (/min)</label>
                        <input type="number" id="respiratoryRate" min="8" max="40" placeholder="16">
                    </div>
                    <div class="form-group">
                        <label for="oxygenSaturation">O2 Saturation (%)</label>
                        <input type="number" id="oxygenSaturation" min="70" max="100" placeholder="98">
                    </div>
                </div>
                
                <div class="form-row">
                    <div class="form-group">
                        <label for="painScore">Pain Score (0-10)</label>
                        <input type="number" id="painScore" min="0" max="10" placeholder="2">
                    </div>
                    <div class="form-group">
                        <label for="urineOutput">Urine Output (mL)</label>
                        <input type="number" id="urineOutput" min="0" max="5000" placeholder="1500">
                    </div>
                    <div class="form-group">
                        <label for="drainOutput">Drain Output (mL)</label>
                        <input type="number" id="drainOutput" min="0" max="1000" placeholder="0">
                    </div>
                </div>
            </div>
            
            <!-- Clinical Notes -->
            <div class="form-group">
                <label for="progressNote">Progress Note</label>
                <textarea id="progressNote" placeholder="Patient's general condition, symptoms, any concerns..." style="height: 100px;"></textarea>
            </div>
            
            <div class="form-group">
                <label for="medicationsGiven">Medications Given</label>
                <textarea id="medicationsGiven" placeholder="List medications administered today..."></textarea>
            </div>
            
            <div class="form-group">
                <label for="clinicalNotes">Additional Notes</label>
                <textarea id="clinicalNotes" placeholder="Any additional observations or notes..."></textarea>
            </div>
            
            <div style="text-align: right; margin-top: 30px;">
                <button type="button" id="cancelDailyLog" class="btn btn-secondary">Cancel</button>
                <button type="submit" class="btn btn-primary">Save Daily Log</button>
            </div>
            
            <div id="dailyLogError" class="error-message"></div>
            <div id="dailyLogSuccess" class="success-message"></div>
        </form>
    </div>
</div>
        
            <!-- View Operative Notes -->
            <div id="viewOperativeNotesView" class="hidden">
                <div class="form-container">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
                        <h2>🔪 Operative Notes</h2>
                        <button id="backFromOperativeNotes" class="btn btn-primary">← Back to Patient</button>
                    </div>
                    
                    <div id="operativeNotesList">
                        <div class="loading">
                            <div class="spinner"></div>
                            <p>Loading operative notes...</p>
                        </div>
                    </div>
                </div>
    </div>
</div>

    <script>
        // Firebase Configuration
        const firebaseConfig = {
            apiKey: "AIzaSyCaKJgggVlJYLl6j6klgKUuEc3NmGFQZv4",
            authDomain: "urology-app-6f2d2.firebaseapp.com",
            projectId: "urology-app-6f2d2",
            storageBucket: "urology-app-6f2d2.firebasestorage.app",
            messagingSenderId: "996503565094",
            appId: "1:996503565094:web:7aed31cf47abd380ab07e7"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.firestore();
        
        // Global variables
        let currentUser = null;
        let userRole = null;
        let allPatients = [];
        
        // DOM elements
        const loadingScreen = document.getElementById('loadingScreen');
        const loginScreen = document.getElementById('loginScreen');
        const mainApp = document.getElementById('mainApp');
        const loginForm = document.getElementById('loginForm');
        const loginError = document.getElementById('loginError');
        const logoutBtn = document.getElementById('logoutBtn');
        const userEmail = document.getElementById('userEmail');
        const userRoleSpan = document.getElementById('userRole');
        
        // Navigation elements
        const showPatientsBtn = document.getElementById('showPatientsBtn');
        const showAddPatientBtn = document.getElementById('showAddPatientBtn');
        const patientsView = document.getElementById('patientsView');
        const addPatientView = document.getElementById('addPatientView');
        
        // Patient management elements
        const patientsList = document.getElementById('patientsList');
        const patientCount = document.getElementById('patientCount');
        const searchInput = document.getElementById('searchInput');
        const statusFilter = document.getElementById('statusFilter');
        const addPatientForm = document.getElementById('addPatientForm');
        const cancelAddPatient = document.getElementById('cancelAddPatient');
        const addPatientError = document.getElementById('addPatientError');
        const addPatientSuccess = document.getElementById('addPatientSuccess');
        
        // Show/hide screens
        function showScreen(screenName) {
            loadingScreen.classList.add('hidden');
            loginScreen.classList.add('hidden');
            mainApp.classList.add('hidden');
            
            if (screenName === 'loading') {
                loadingScreen.classList.remove('hidden');
            } else if (screenName === 'login') {
                loginScreen.classList.remove('hidden');
            } else if (screenName === 'main') {
                mainApp.classList.remove('hidden');
            }
        }

        // Update showView function to handle all views
          function showView(viewName) {
           // Hide all views
           const views = [
               'patientsView', 'addPatientView', 'patientDetailView', 'editPatientView',
               'operativeNoteView', 'viewOperativeNotesView'
           ];
           
           views.forEach(viewId => {
               const element = document.getElementById(viewId);
               if (element) element.classList.add('hidden');
           });
    
           // Reset button states
           const showPatientsBtn = document.getElementById('showPatientsBtn');
           const showAddPatientBtn = document.getElementById('showAddPatientBtn');
           if (showPatientsBtn) showPatientsBtn.classList.remove('btn-primary');
           if (showAddPatientBtn) showAddPatientBtn.classList.remove('btn-primary');
    
           // Show requested view
           if (viewName === 'patients') {
               const patientsView = document.getElementById('patientsView');
               if (patientsView) patientsView.classList.remove('hidden');
               if (showPatientsBtn) showPatientsBtn.classList.add('btn-primary');
           } else if (viewName === 'addPatient') {
               const addPatientView = document.getElementById('addPatientView');
               if (addPatientView) addPatientView.classList.remove('hidden');
               if (showAddPatientBtn) showAddPatientBtn.classList.add('btn-primary');
           } else if (viewName === 'patientDetail') {
               const patientDetailView = document.getElementById('patientDetailView');
               if (patientDetailView) patientDetailView.classList.remove('hidden');
           } else if (viewName === 'editPatient') {
               const editPatientView = document.getElementById('editPatientView');
               if (editPatientView) editPatientView.classList.remove('hidden');
           } else if (viewName === 'operativeNote') {
               const operativeNoteView = document.getElementById('operativeNoteView');
               if (operativeNoteView) operativeNoteView.classList.remove('hidden');
           } else if (viewName === 'viewOperativeNotes') {
               const viewOperativeNotesView = document.getElementById('viewOperativeNotesView');
               if (viewOperativeNotesView) viewOperativeNotesView.classList.remove('hidden');
           }
       }
        
        // Show error/success messages
        function showMessage(elementId, message, isSuccess = false) {
            const element = document.getElementById(elementId);
            element.textContent = message;
            element.style.display = 'block';
            
            setTimeout(() => {
                element.style.display = 'none';
            }, 5000);
        }
        
        // Authentication state listener
        auth.onAuthStateChanged(async (user) => {
            if (user) {
                currentUser = user;
                
                try {
                    const userDoc = await db.collection('users').doc(user.uid).get();
                    if (userDoc.exists) {
                        const userData = userDoc.data();
                        userRole = userData.role;
                        userEmail.textContent = userData.email;
                        userRoleSpan.textContent = userData.role.toUpperCase();
                        
                        showScreen('main');
                        showView('patients');
                        loadPatients();
                        
                        // Set today's date as default
                        document.getElementById('admissionDate').valueAsDate = new Date();
                    } else {
                        showMessage('loginError', 'User role not found. Please contact administrator.');
                        auth.signOut();
                    }
                } catch (error) {
                    console.error('Error getting user data:', error);
                    showMessage('loginError', 'Error loading user data.');
                    auth.signOut();
                }
            } else {
                currentUser = null;
                userRole = null;
                showScreen('login');
            }
        });
        
        // Login form handler
        loginForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            
            try {
                await auth.signInWithEmailAndPassword(email, password);
            } catch (error) {
                console.error('Login error:', error);
                showMessage('loginError', 'Invalid email or password.');
            }
        });
        
        // Logout handler
        logoutBtn.addEventListener('click', () => {
            auth.signOut();
        });
        
        // Navigation handlers
        showPatientsBtn.addEventListener('click', () => {
            showView('patients');
            loadPatients();
        });
        
        showAddPatientBtn.addEventListener('click', () => {
            showView('addPatient');
            addPatientForm.reset();
            document.getElementById('admissionDate').valueAsDate = new Date();
        });
        
        cancelAddPatient.addEventListener('click', () => {
            showView('patients');
        });
        
        // Load patients from Firestore
        async function loadPatients() {
            try {
                patientsList.innerHTML = '<div class="loading"><div class="spinner"></div><p>Loading patients...</p></div>';
                
                const snapshot = await db.collection('patients').orderBy('createdAt', 'desc').get();
                allPatients = [];
                
                snapshot.forEach(doc => {
                    allPatients.push({
                        id: doc.id,
                        ...doc.data()
                    });
                });
                
                displayPatients(allPatients);
                
            } catch (error) {
                console.error('Error loading patients:', error);
                patientsList.innerHTML = '<p style="text-align: center; color: #f44336;">Error loading patients. Please try again.</p>';
            }
        }
        
        // Display patients list
        function displayPatients(patients) {
            patientCount.textContent = patients.length;
            
            if (patients.length === 0) {
                patientsList.innerHTML = '<p style="text-align: center; color: #666;">No patients found. Click "Add New Patient" to get started.</p>';
                return;
            }
            
            patientsList.innerHTML = patients.map(patient => `
                <div class="patient-card">
                    <div style="display: flex; justify-content: space-between; align-items: start;">
                        <h3>${patient.name || 'Unnamed Patient'}</h3>
                        <span class="status-badge status-${patient.status || 'pre-op'}">${patient.status || 'pre-op'}</span>
                    </div>
                    <div class="patient-info">
                        <span><strong>MRN:</strong> ${patient.mrn || 'N/A'}</span>
                        <span><strong>Age:</strong> ${patient.age || 'N/A'}</span>
                        <span><strong>Gender:</strong> ${patient.gender || 'N/A'}</span>
                        <span><strong>Room:</strong> ${patient.roomNumber || 'N/A'}</span>
                        <span><strong>Admission:</strong> ${patient.admissionDate || 'N/A'}</span>
                        <span><strong>Phone:</strong> ${patient.phone || 'N/A'}</span>
                    </div>
                    <div class="patient-info">
                        <span><strong>Complaint:</strong> ${(patient.presentingComplaint || 'N/A').substring(0, 100)}${(patient.presentingComplaint || '').length > 100 ? '...' : ''}</span>
                    </div>
                    <div class="patient-actions">
                        <button class="btn btn-primary btn-sm" onclick="viewPatient('${patient.id}')">📋 View Details</button>
                        <button class="btn btn-secondary btn-sm" onclick="editPatient('${patient.id}')">✏️ Edit</button>
                    </div>
                </div>
            `).join('');
        }
        
        // Search and filter functionality
        searchInput.addEventListener('input', filterPatients);
        statusFilter.addEventListener('change', filterPatients);
        
        function filterPatients() {
            const searchTerm = searchInput.value.toLowerCase();
            const statusFilter = document.getElementById('statusFilter').value;
            
            let filteredPatients = allPatients.filter(patient => {
                const matchesSearch = !searchTerm || 
                    (patient.name || '').toLowerCase().includes(searchTerm) ||
                    (patient.mrn || '').toLowerCase().includes(searchTerm) ||
                    (patient.roomNumber || '').toLowerCase().includes(searchTerm);
                
                const matchesStatus = !statusFilter || patient.status === statusFilter;
                
                return matchesSearch && matchesStatus;
            });
            
            displayPatients(filteredPatients);
        }
        
        // Add patient form handler
        addPatientForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            try {
                const patientData = {
                    name: document.getElementById('patientName').value,
                    mrn: document.getElementById('patientMRN').value,
                    age: parseInt(document.getElementById('patientAge').value) || null,
                    gender: document.getElementById('patientGender').value,
                    phone: document.getElementById('patientPhone').value,
                    roomNumber: document.getElementById('roomNumber').value,
                    admissionDate: document.getElementById('admissionDate').value,
                    presentingComplaint: document.getElementById('presentingComplaint').value,
                    pmh: document.getElementById('pmh').value,
                    allergies: document.getElementById('allergies').value,
                    medications: document.getElementById('medications').value,
                    physicalExam: document.getElementById('physicalExam').value,
                    imagingSummary: document.getElementById('imagingSummary').value,
                    labsSummary: document.getElementById('labsSummary').value,
                    status: 'pre-op',
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    createdBy: currentUser.uid,
                    createdByName: userEmail.textContent
                };
                
                await db.collection('patients').add(patientData);
                
                showMessage('addPatientSuccess', 'Patient added successfully!', true);
                addPatientForm.reset();
                document.getElementById('admissionDate').valueAsDate = new Date();
                
                setTimeout(() => {
                    showView('patients');
                    loadPatients();
                }, 2000);
                
            } catch (error) {
                console.error('Error adding patient:', error);
                showMessage('addPatientError', 'Error adding patient. Please try again.');
            }
        });
        
        // Placeholder functions for future features
       // Global variable for current patient
        let currentPatientId = null;
        let currentPatientData = null;
        
        // DOM elements for new views
        const patientDetailView = document.getElementById('patientDetailView');
        const editPatientView = document.getElementById('editPatientView');
        const patientDetailName = document.getElementById('patientDetailName');
        const patientDetailContent = document.getElementById('patientDetailContent');
        const editPatientBtn = document.getElementById('editPatientBtn');
        const backToPatientsBtn = document.getElementById('backToPatientsBtn');
        const editPatientForm = document.getElementById('editPatientForm');
        const cancelEditPatient = document.getElementById('cancelEditPatient');
        const deletePatientBtn = document.getElementById('deletePatientBtn');
        
        // View patient details
        async function viewPatient(patientId) {
            try {
                currentPatientId = patientId;
                showView('patientDetail');
                
                // Load patient data
                const doc = await db.collection('patients').doc(patientId).get();
                if (doc.exists) {
                    currentPatientData = { id: doc.id, ...doc.data() };
                    displayPatientDetail(currentPatientData);
                } else {
                    alert('Patient not found');
                    showView('patients');
                }
            } catch (error) {
                console.error('Error loading patient:', error);
                alert('Error loading patient details');
                showView('patients');
            }
        }
        
        // Display patient detail
        function displayPatientDetail(patient) {
            patientDetailName.textContent = patient.name || 'Unnamed Patient';
            
            const formatDate = (dateStr) => {
                if (!dateStr) return 'N/A';
                try {
                    return new Date(dateStr).toLocaleDateString();
                } catch {
                    return dateStr;
                }
            };
            
            patientDetailContent.innerHTML = `
                <div class="patient-header">
                    <div>
                        <h2>${patient.name || 'Unnamed Patient'}</h2>
                        <div class="patient-meta">
                            <span>MRN: ${patient.mrn || 'N/A'}</span>
                            <span>Age: ${patient.age || 'N/A'}</span>
                            <span>Gender: ${patient.gender || 'N/A'}</span>
                            <span class="status-badge status-${patient.status || 'pre-op'}">${patient.status || 'pre-op'}</span>
                        </div>
                    </div>
                </div>
                
                <div class="patient-detail-grid">
                    <div class="detail-section">
                        <h3>📋 Basic Information</h3>
                        <p><strong>Phone:</strong> ${patient.phone || 'N/A'}</p>
                        <p><strong>Room:</strong> ${patient.roomNumber || 'N/A'}</p>
                        <p><strong>Admission:</strong> ${formatDate(patient.admissionDate)}</p>
                        <p><strong>Created by:</strong> ${patient.createdByName || 'Unknown'}</p>
                    </div>
                    
                    <div class="detail-section">
                        <h3>🩺 Medical History</h3>
                        <p><strong>Allergies:</strong> ${patient.allergies || 'None reported'}</p>
                        <p><strong>Medications:</strong> ${patient.medications || 'None listed'}</p>
                        <p><strong>PMH:</strong> ${patient.pmh || 'Not documented'}</p>
                    </div>
                    
                    <div class="detail-section">
                        <h3>🔍 Presenting Complaint</h3>
                        <p>${patient.presentingComplaint || 'Not documented'}</p>
                    </div>
                    
                    <div class="detail-section">
                        <h3>👥 Physical Examination</h3>
                        <p>${patient.physicalExam || 'Not documented'}</p>
                    </div>
                    
                    <div class="detail-section">
                        <h3>🩻 Imaging Summary</h3>
                        <p>${patient.imagingSummary || 'No imaging documented'}</p>
                    </div>
                    
                    <div class="detail-section">
                        <h3>🧪 Laboratory Results</h3>
                        <p>${patient.labsSummary || 'No labs documented'}</p>
                    </div>
                
                <div class="detail-section" style="grid-column: 1 / -1;">
                    <h3>🔪 Operative Notes</h3>
                    <div id="operativeNotesContainer">
                        <div class="loading" style="text-align: center; padding: 20px;">
                            <div class="spinner" style="width: 20px; height: 20px;"></div>
                            <p>Loading operative notes...</p>
                        </div>
                    </div>

                                    
                </div>
            `;
                    
        // Load operative notes
        loadOperativeNotes(patient.id);
        }

        // Load and display operative notes
        async function loadOperativeNotes(patientId) {
            try {
                const container = document.getElementById('operativeNotesContainer');
                
                const snapshot = await db.collection('patients').doc(patientId).collection('operativeNotes').orderBy('createdAt', 'desc').get();
                
                if (snapshot.empty) {
                    container.innerHTML = '<p style="color: #666; font-style: italic;">No operative notes recorded yet.</p>';
                    return;
                }
                
                let notesHtml = '';
                snapshot.forEach(doc => {
                    const note = doc.data();
                    const date = note.createdAt ? note.createdAt.toDate().toLocaleDateString() : 'Unknown date';
                    const time = note.createdAt ? note.createdAt.toDate().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) : '';
                    
                    notesHtml += `
                        <div class="operative-note-card" style="margin-bottom: 15px;">
                            <div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 15px;">
                                <h4 style="color: #2196F3; margin: 0;">${note.surgeryType || 'Operative Procedure'}</h4>
                                <span style="font-size: 12px; color: #888;">${date} ${time}</span>
                            </div>
                            
                            <div class="operative-note-header">
                                <span><strong>Date:</strong> ${note.surgeryDate || 'N/A'}</span>
                                <span><strong>Surgeon:</strong> ${note.surgeon || 'N/A'}</span>
                                <span><strong>Anesthesia:</strong> ${note.anesthesiaType || 'N/A'}</span>
                                <span><strong>Blood Loss:</strong> ${note.bloodLoss || 'N/A'}</span>
                            </div>
                            
                            <div class="operative-section">
                                <h4>Findings</h4>
                                <p>${note.findings || 'Not documented'}</p>
                            </div>
                            
                            <div class="operative-section">
                                <h4>Operative Steps</h4>
                                <p>${note.operativeSteps || 'Not documented'}</p>
                            </div>
                            
                            <div class="operative-section">
                                <h4>Complications</h4>
                                <p>${note.complications || 'None reported'}</p>
                            </div>
                            
                            <div class="operative-note-meta">
                                <span>Created by: ${note.createdByName || 'Unknown'}</span>
                                <div class="operative-note-actions">
                                    <button class="btn btn-sm btn-secondary" onclick="editOperativeNote('${doc.id}', '${patientId}')" style="padding: 4px 8px; font-size: 12px;">
                                        ✏️ Edit
                                    </button>
                                    <button class="btn btn-sm btn-danger" onclick="deleteOperativeNote('${doc.id}', '${patientId}')" style="padding: 4px 8px; font-size: 12px;">
                                        🗑️ Delete
                                    </button>
                                </div>
                            </div>
                        </div>
                    `;
                });
                
                container.innerHTML = notesHtml;
                
            } catch (error) {
                console.error('Error loading operative notes:', error);
                document.getElementById('operativeNotesContainer').innerHTML = 
                    '<p style="color: #f44336;">Error loading operative notes.</p>';
            }
        }

        // Global variable for editing operative notes
        let currentOperativeNoteId = null;
        let editingOperativeNote = false;
        
        // Edit operative note
        async function editOperativeNote(noteId, patientId) {
            try {
                currentOperativeNoteId = noteId;
                editingOperativeNote = true;
                
                // Load the operative note data
                const doc = await db.collection('patients').doc(patientId).collection('operativeNotes').doc(noteId).get();
                
                if (doc.exists) {
                    const note = doc.data();
                    
                    // Switch to operative note view
                    showView('operativeNote');
                    
                    // Fill form with existing data
                    document.getElementById('surgeryType').value = note.surgeryType || '';
                    document.getElementById('surgeryDate').value = note.surgeryDate || '';
                    document.getElementById('surgeryTime').value = note.surgeryTime || '';
                    document.getElementById('surgeon').value = note.surgeon || '';
                    document.getElementById('assistants').value = note.assistants || '';
                    document.getElementById('anesthesiaType').value = note.anesthesiaType || '';
                    document.getElementById('position').value = note.position || '';
                    document.getElementById('findings').value = note.findings || '';
                    document.getElementById('operativeSteps').value = note.operativeSteps || '';
                    document.getElementById('complications').value = note.complications || '';
                    document.getElementById('bloodLoss').value = note.bloodLoss || '';
                    
                    // Change form title and button text
                    document.querySelector('#operativeNoteView h2').textContent = '✏️ Edit Operative Note';
                    document.querySelector('#operativeNoteForm button[type="submit"]').textContent = 'Update Operative Note';
                    
                } else {
                    alert('Operative note not found');
                }
                
            } catch (error) {
                console.error('Error loading operative note for editing:', error);
                alert('Error loading operative note');
            }
        }
        
        // Delete operative note
        async function deleteOperativeNote(noteId, patientId) {
            if (confirm('Are you sure you want to delete this operative note? This action cannot be undone.')) {
                try {
                    await db.collection('patients').doc(patientId).collection('operativeNotes').doc(noteId).delete();
                    
                    // Reload operative notes to refresh the display
                    loadOperativeNotes(patientId);
                    
                    alert('Operative note deleted successfully');
                    
                } catch (error) {
                    console.error('Error deleting operative note:', error);
                    alert('Error deleting operative note');
                }
            }
        }
        
        // Reset operative note form to add mode
        function resetOperativeNoteForm() {
            currentOperativeNoteId = null;
            editingOperativeNote = false;
            
            // Reset form title and button text
            document.querySelector('#operativeNoteView h2').textContent = '🔪 Add Operative Note';
            document.querySelector('#operativeNoteForm button[type="submit"]').textContent = 'Save Operative Note';
            
            // Clear form
            operativeNoteForm.reset();
            document.getElementById('surgeryDate').valueAsDate = new Date();
            document.getElementById('surgeon').value = userEmail.textContent.split('@')[0].replace('.', ' ');
            
            // Remove active state from template buttons
            document.querySelectorAll('.template-btn').forEach(btn => {
                btn.classList.remove('active');
            });
        }
        
        // Edit patient
        function editPatient(patientId) {
            if (patientId) {
                currentPatientId = patientId;
                // Load patient data first if not already loaded
                if (!currentPatientData || currentPatientData.id !== patientId) {
                    viewPatient(patientId).then(() => {
                        populateEditForm();
                        showView('editPatient');
                    });
                } else {
                    populateEditForm();
                    showView('editPatient');
                }
            } else if (currentPatientData) {
                populateEditForm();
                showView('editPatient');
            }
        }
        
        // Populate edit form with current patient data
        function populateEditForm() {
            if (!currentPatientData) return;
            
            document.getElementById('editPatientName').value = currentPatientData.name || '';
            document.getElementById('editPatientMRN').value = currentPatientData.mrn || '';
            document.getElementById('editPatientAge').value = currentPatientData.age || '';
            document.getElementById('editPatientGender').value = currentPatientData.gender || '';
            document.getElementById('editPatientPhone').value = currentPatientData.phone || '';
            document.getElementById('editRoomNumber').value = currentPatientData.roomNumber || '';
            document.getElementById('editAdmissionDate').value = currentPatientData.admissionDate || '';
            document.getElementById('editPatientStatus').value = currentPatientData.status || 'pre-op';
            document.getElementById('editPresentingComplaint').value = currentPatientData.presentingComplaint || '';
            document.getElementById('editPmh').value = currentPatientData.pmh || '';
            document.getElementById('editAllergies').value = currentPatientData.allergies || '';
            document.getElementById('editMedications').value = currentPatientData.medications || '';
            document.getElementById('editPhysicalExam').value = currentPatientData.physicalExam || '';
            document.getElementById('editImagingSummary').value = currentPatientData.imagingSummary || '';
            document.getElementById('editLabsSummary').value = currentPatientData.labsSummary || '';
        }
        
        // Delete patient with confirmation
        function confirmDeletePatient() {
            if (!currentPatientData) return;
            
            const confirmDialog = document.createElement('div');
            confirmDialog.className = 'confirmation-dialog';
            confirmDialog.innerHTML = `
                <div class="confirmation-content">
                    <h3>⚠️ Delete Patient</h3>
                    <p>Are you sure you want to delete <strong>${currentPatientData.name}</strong>?</p>
                    <p style="color: #f44336; font-size: 14px;">This action cannot be undone!</p>
                    <div>
                        <button class="btn btn-secondary" onclick="closeConfirmDialog()">Cancel</button>
                        <button class="btn btn-danger" onclick="deletePatientConfirmed()" style="margin-left: 10px;">Delete</button>
                    </div>
                </div>
            `;
            
            document.body.appendChild(confirmDialog);
        }
        
        // Close confirmation dialog
        function closeConfirmDialog() {
            const dialog = document.querySelector('.confirmation-dialog');
            if (dialog) {
                dialog.remove();
            }
        }
        
        // Actually delete the patient
        async function deletePatientConfirmed() {
            try {
                await db.collection('patients').doc(currentPatientId).delete();
                closeConfirmDialog();
                showView('patients');
                loadPatients();
                alert('Patient deleted successfully');
            } catch (error) {
                console.error('Error deleting patient:', error);
                alert('Error deleting patient');
            }
        }
        
        // Event listeners for new buttons
        editPatientBtn.addEventListener('click', () => editPatient());
        backToPatientsBtn.addEventListener('click', () => showView('patients'));
        cancelEditPatient.addEventListener('click', () => showView('patientDetail'));
        deletePatientBtn.addEventListener('click', confirmDeletePatient);
        
        // Edit patient form handler
        editPatientForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            try {
                const updatedData = {
                    name: document.getElementById('editPatientName').value,
                    mrn: document.getElementById('editPatientMRN').value,
                    age: parseInt(document.getElementById('editPatientAge').value) || null,
                    gender: document.getElementById('editPatientGender').value,
                    phone: document.getElementById('editPatientPhone').value,
                    roomNumber: document.getElementById('editRoomNumber').value,
                    admissionDate: document.getElementById('editAdmissionDate').value,
                    status: document.getElementById('editPatientStatus').value,
                    presentingComplaint: document.getElementById('editPresentingComplaint').value,
                    pmh: document.getElementById('editPmh').value,
                    allergies: document.getElementById('editAllergies').value,
                    medications: document.getElementById('editMedications').value,
                    physicalExam: document.getElementById('editPhysicalExam').value,
                    imagingSummary: document.getElementById('editImagingSummary').value,
                    labsSummary: document.getElementById('editLabsSummary').value,
                    updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
                    updatedBy: currentUser.uid,
                    updatedByName: userEmail.textContent
                };
                
                await db.collection('patients').doc(currentPatientId).update(updatedData);
                
                showMessage('editPatientSuccess', 'Patient updated successfully!', true);
                
                // Update current patient data
                currentPatientData = { ...currentPatientData, ...updatedData };
                
                setTimeout(() => {
                    showView('patientDetail');
                    displayPatientDetail(currentPatientData);
                    loadPatients(); // Refresh the list
                }, 2000);
                
            } catch (error) {
                console.error('Error updating patient:', error);
                showMessage('editPatientError', 'Error updating patient. Please try again.');
            }
        });
        
        // Placeholder functions for future features (Part 4)
        document.getElementById('createDischargeBtn').addEventListener('click', () => {
            alert('Discharge Summary feature will be added in Part 6!');
        });
        
        // Surgery Templates Database
        const surgeryTemplates = {
            urs_laser: {
                name: "Ureteric Stone Extraction with Thulium Laser Lithotripsy",
                surgeryType: "Right/Left ureteroscopic stone extraction with Thulium laser lithotripsy and DJ stent placement",
                anesthesiaType: "General anesthesia",
                position: "Lithotomy position",
                findings: "A single/multiple stone(s) were identified in the mid/distal ureter. The ureter was mildly edematous. No evidence of perforation or extravasation was noted.",
                operativeSteps: `1. After induction of general anesthesia, the patient was placed in lithotomy position and prepped and draped in sterile fashion.
2. A safety guidewire was introduced into the ureter under cystoscopic guidance.
3. A semi-rigid ureteroscope was advanced to visualize the ureteric stone.
4. Thulium laser lithotripsy was performed to dust and fragment the stone.
5. Fragments were retrieved with a basket as needed.
6. A retrograde pyelogram was performed to assess the upper tract and confirm integrity.
7. A 5/6 Fr DJ stent was inserted over the guidewire with the distal curl in the bladder and proximal coil in the renal pelvis.
8. Hemostasis was confirmed, and instruments were withdrawn.`,
                complications: "None encountered intraoperatively",
                bloodLoss: "<50 mL"
            },
            flexible_urs: {
                name: "Flexible Ureteroscopy (URS) for Renal Stones",
                surgeryType: "Unilateral/Bilateral flexible ureteroscopy with laser lithotripsy and DJ stent placement",
                anesthesiaType: "General anesthesia",
                position: "Lithotomy position",
                findings: "Multiple/single stone(s) noted within the renal pelvis and calyces, most commonly lower pole. Mucosa was intact with mild edema. No extravasation or ureteral injury observed.",
                operativeSteps: `1. Under general anesthesia, the patient was placed in the lithotomy position and prepped and draped in sterile fashion.
2. A safety guidewire was inserted into the ureter under cystoscopic guidance.
3. A ureteral access sheath was introduced where feasible.
4. A flexible ureteroscope was advanced to the renal pelvis, and all calyces were systematically inspected.
5. Stones were visualized and fragmented using Thulium laser (dusting and/or fragmentation technique).
6. Fragments were actively retrieved using a nitinol basket where appropriate.
7. Complete inspection of all calyces was performed to exclude residual fragments.
8. A 5/6 Fr DJ stent was inserted in the treated side(s) under fluoroscopic or endoscopic guidance.
9. Hemostasis was verified, and all instruments were removed.`,
                complications: "None encountered intraoperatively",
                bloodLoss: "<50 mL"
            },
            turp: {
                name: "Transurethral Resection of the Prostate (TURP)",
                surgeryType: "Transurethral resection of the prostate (TURP)",
                anesthesiaType: "Spinal anesthesia",
                position: "Lithotomy position",
                findings: "Enlarged prostate gland with prominent lateral and median lobes protruding into the bladder. Mild trabeculation of bladder noted. No bladder stones or tumors observed. Ureteric orifices identified bilaterally and preserved.",
                operativeSteps: `1. Under spinal anesthesia, the patient was placed in the lithotomy position, prepped, and draped in sterile fashion.
2. Initial cystoscopy was performed to assess the bladder and visualize the prostatic enlargement and ureteric orifices.
3. A monopolar/bipolar resectoscope was introduced.
4. Resection began at the median lobe, followed by the lateral lobes, proceeding in a systematic fashion down to the surgical capsule.
5. Bladder neck was resected and smoothed.
6. Complete hemostasis was achieved using coagulation current.
7. Prostatic chips were evacuated using an Ellik evacuator.
8. A 22/24 Fr three-way Foley catheter was inserted and continuous bladder irrigation was started.`,
                complications: "None encountered intraoperatively",
                bloodLoss: "Approximately 200 mL"
            },
            orchiopexy: {
                name: "Orchiopexy for Undescended Testis",
                surgeryType: "Right/Left inguinal exploration and orchiopexy",
                anesthesiaType: "General anesthesia",
                position: "Supine position",
                findings: "Testis found in the inguinal canal/at the internal ring. Spermatic cord of adequate length with no significant tension after mobilization. Hernial sac present/absent.",
                operativeSteps: `1. Under general anesthesia, the patient was placed in the supine position and prepped and draped in sterile fashion.
2. A standard inguinal incision was made over the inguinal canal.
3. The external oblique aponeurosis was opened, and the cord structures were isolated.
4. The undescended testis was identified and mobilized with high dissection of the cord structures.
5. A patent processus vaginalis was identified and ligated at the level of the internal ring (if present).
6. The testis was brought down to the scrotum through a subdartos pouch via a separate scrotal incision.
7. The testis was fixed in position using absorbable sutures.
8. Hemostasis was ensured, and the inguinal and scrotal wounds were closed in layers.`,
                complications: "None encountered intraoperatively",
                bloodLoss: "Minimal (<20 mL)"
            },
            circumcision: {
                name: "Surgical Circumcision",
                surgeryType: "Circumcision using sleeve technique",
                anesthesiaType: "Local anesthesia with/without sedation",
                position: "Supine position",
                findings: "Phimosis/paraphimosis/redundant foreskin. No signs of infection or inflammation. Glans penis appeared healthy and intact.",
                operativeSteps: `1. The patient was placed in the supine position and prepped and draped in sterile fashion.
2. Local anesthetic was infiltrated at the base of the penis (penile block) with or without dorsal nerve block.
3. The preputial adhesions were released and the foreskin fully retracted.
4. Markings were made for the sleeve excision at the junction of inner and outer prepuce.
5. The foreskin was incised circumferentially and excised carefully, preserving the glans and frenulum.
6. Hemostasis was achieved with electrocautery or ligatures.
7. The edges of the skin were approximated with interrupted or running absorbable sutures (e.g., 4-0 Vicryl Rapide).
8. Sterile dressing was applied.`,
                complications: "None encountered intraoperatively",
                bloodLoss: "Minimal (<10 mL)"
            },
            pcnl: {
                name: "Percutaneous Nephrolithotomy (PCNL)",
                surgeryType: "Percutaneous nephrolithotomy – Right/Left kidney",
                anesthesiaType: "General anesthesia",
                position: "Initial lithotomy, then prone position",
                findings: "Large renal calculus occupying the renal pelvis with/without extension into calyces. Moderate hydronephrosis. No collecting system perforation or bleeding encountered during the procedure.",
                operativeSteps: `1. Under general anesthesia, the patient was initially positioned in lithotomy position and prepped and draped in sterile fashion.
2. A cystoscopy was performed and a 5–6 Fr open-ended ureteric catheter was advanced into the renal pelvis under fluoroscopic guidance.
3. The catheter was secured and connected to contrast for subsequent pelvicalyceal system opacification.
4. The patient was then repositioned into prone position and re-prepped and draped.
5. Percutaneous access was obtained into a posterior lower/middle calyx under fluoroscopic (or ultrasound) guidance.
6. The tract was dilated using serial fascial dilators or a balloon dilator, and a 30 Fr Amplatz sheath was placed.
7. A rigid nephroscope was introduced through the sheath.
8. Stones were visualized and fragmented using ultrasonic/pneumatic lithotripter or laser.
9. Fragments were extracted using forceps or suction, and a flexible nephroscope was used to inspect all calyces for residual stones.
10. A nephrostomy tube was inserted. DJ stent was placed if indicated.
11. Hemostasis was confirmed and dressing applied.`,
                complications: "None encountered intraoperatively",
                bloodLoss: "Approximately 100–300 mL"
            },
            other: {
                name: "Other Surgery (Manual Entry)",
                surgeryType: "",
                anesthesiaType: "",
                position: "",
                findings: "",
                operativeSteps: "",
                complications: "",
                bloodLoss: ""
            }
        };
        
        // Add more templates for the remaining surgeries
        surgeryTemplates.inguinal_hernia = {
            name: "Inguinal Hernia Repair (Open Hernioplasty)",
            surgeryType: "Right/Left inguinal hernia repair with mesh (Lichtenstein hernioplasty)",
            anesthesiaType: "Spinal anesthesia",
            position: "Supine position",
            findings: "Indirect/direct inguinal hernia sac identified. No bowel compromise. Hernia contents reducible. Spermatic cord and surrounding structures preserved.",
            operativeSteps: `1. Under spinal anesthesia, the patient was placed in the supine position and prepped and draped in sterile fashion.
2. An oblique inguinal incision was made, and the external oblique aponeurosis was incised.
3. The hernia sac was identified and dissected from the cord structures.
4. The sac was opened, contents inspected and reduced into the peritoneal cavity.
5. The sac was transfixed and excised.
6. A polypropylene mesh was fashioned and placed over the posterior wall of the inguinal canal, covering the deep ring.
7. The mesh was fixed to the pubic tubercle medially, to the inguinal ligament inferiorly, and to the conjoint tendon superiorly.
8. The external oblique aponeurosis was closed over the cord, followed by skin closure.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "<50 mL"
        };
        
        surgeryTemplates.pediatric_hernia = {
            name: "Pediatric Inguinal Hernia Repair (Herniotomy)",
            surgeryType: "Right/Left inguinal herniotomy",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Indirect hernia sac extending through the deep inguinal ring. Hernia contents reducible. No ischemic changes observed. Testis and cord structures were normal.",
            operativeSteps: `1. Under general anesthesia, the patient was positioned supine and prepped and draped in a sterile fashion.
2. A transverse skin crease incision was made over the inguinal region.
3. The external oblique aponeurosis was incised, and the inguinal canal was opened.
4. The hernia sac was identified lateral to the cord structures and carefully dissected free.
5. The sac was opened, and its contents were reduced into the peritoneal cavity.
6. The sac was transfixed and ligated high at the level of the internal ring using absorbable suture.
7. Excess sac distal to the ligature was excised.
8. The inguinal canal was closed, and the wound was closed in layers with absorbable sutures.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<10 mL)"
        };

        surgeryTemplates.hydrocelectomy = {
            name: "Hydrocelectomy",
            surgeryType: "Right/Left hydrocelectomy (Jaboulay procedure)",
            anesthesiaType: "Spinal anesthesia",
            position: "Supine position",
            findings: "Large/simple hydrocele involving the tunica vaginalis. Clear/straw-colored fluid encountered. No thickening or suspicious pathology of the tunica or testis. Spermatic cord structures normal.",
            operativeSteps: `1. After induction of spinal anesthesia, the patient was positioned supine and prepped and draped in sterile fashion.
2. A transverse scrotal incision was made over the hydrocele.
3. Dissection was carried down to the tunica vaginalis.
4. The hydrocele sac was opened, and fluid was drained.
5. The sac was inspected and everted (Jaboulay technique), and redundant tunica was excised if needed.
6. Hemostasis was secured.
7. The testis was replaced into the scrotum, and the dartos and skin layers were closed using absorbable sutures.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<20 mL)"
        };
        
        surgeryTemplates.varicocelectomy = {
            name: "Microscopic Varicocelectomy",
            surgeryType: "Subinguinal microscopic varicocelectomy – Right/Left side",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Dilated and tortuous pampiniform plexus veins identified. Testicular artery and lymphatics preserved. No evidence of testicular atrophy or abnormal pathology.",
            operativeSteps: `1. Under general anesthesia, the patient was placed in the supine position, prepped, and draped in sterile fashion.
2. A 2–3 cm subinguinal incision was made just below the external inguinal ring.
3. The spermatic cord was identified and isolated on a vascular sling.
4. Under operative microscope magnification (×10–25), the cord structures were carefully dissected.
5. The testicular artery was identified using micro-Doppler and preserved.
6. Dilated veins of the pampiniform plexus were isolated, doubly ligated, and divided.
7. Lymphatic channels were preserved to minimize the risk of postoperative hydrocele.
8. Hemostasis was achieved. The cord was returned to its anatomical position.
9. The incision was closed in layers using absorbable sutures.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<10–20 mL)"
        };
        
        surgeryTemplates.urethrotomy = {
            name: "Optical Internal Urethrotomy",
            surgeryType: "Optical internal urethrotomy for anterior urethral stricture",
            anesthesiaType: "Spinal anesthesia",
            position: "Lithotomy position",
            findings: "A short segment stricture was identified in the bulbar urethra. The urethral mucosa appeared fibrotic at the site of narrowing. No false passages or additional strictures were identified distally.",
            operativeSteps: `1. Under spinal anesthesia, the patient was placed in lithotomy position and prepped and draped in sterile fashion.
2. A cystourethroscope was gently advanced until resistance was encountered at the level of the stricture.
3. The stricture was confirmed endoscopically and a cold-knife urethrotomy was performed at the 12 o'clock position.
4. The incision was extended until the urethral lumen was adequately patent and the bladder was entered.
5. The bladder was inspected and found to be normal.
6. A 16/18 Fr Foley catheter was passed easily into the bladder without resistance.
7. The catheter was secured and left in situ.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<30 mL)"
        };
        
        surgeryTemplates.turbt = {
            name: "Transurethral Resection of Bladder Tumor (TURBT)",
            surgeryType: "Transurethral resection of bladder tumor (TURBT)",
            anesthesiaType: "Spinal anesthesia",
            position: "Lithotomy position",
            findings: "Solitary/multiple papillary sessile tumor(s) located on the bladder wall. Tumor appeared superficial/deep. No gross extravesical extension. Ureteric orifices identified and preserved.",
            operativeSteps: `1. Under spinal anesthesia, the patient was placed in lithotomy position, prepped, and draped in sterile fashion.
2. Diagnostic cystoscopy was performed to localize and characterize the bladder lesion(s).
3. A resectoscope was introduced, and the tumor was resected systematically down to the detrusor muscle.
4. Base of the tumor was resected deeply to include detrusor for histopathologic staging.
5. Bleeding points were coagulated using monopolar/bipolar current.
6. Resected tissue was evacuated using an Ellik evacuator and sent for histopathology.
7. The bladder was thoroughly irrigated, inspected for completeness of resection, and ureteric orifices were visualized.
8. A 22 Fr three-way Foley catheter was inserted, and continuous bladder irrigation was started.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "50–150 mL"
        };
        
        surgeryTemplates.radical_nephrectomy = {
            name: "Radical Nephrectomy",
            surgeryType: "Right/Left open/laparoscopic/robotic radical nephrectomy",
            anesthesiaType: "General anesthesia",
            position: "Flank or lateral decubitus position",
            findings: "Renal mass involving upper/mid/lower pole without evidence of gross local invasion beyond Gerota's fascia. Renal vein and artery visualized. No significant lymphadenopathy or adrenal involvement.",
            operativeSteps: `1. Under general anesthesia, the patient was positioned in lateral decubitus position, prepped, and draped in sterile fashion.
2. A flank incision was made and the colon was reflected medially to expose the kidney.
3. Gerota's fascia was incised and the kidney was mobilized circumferentially.
4. Renal artery and vein were individually dissected, ligated, and divided.
5. The ureter was ligated and divided distally.
6. The entire kidney along with surrounding perinephric fat and Gerota's fascia was removed en bloc.
7. The adrenal gland was preserved or removed based on intraoperative findings.
8. Hemostasis was secured. A drain was placed if indicated.
9. The incision was closed in layers.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "150–400 mL"
        };
        
        surgeryTemplates.partial_nephrectomy = {
            name: "Partial Nephrectomy",
            surgeryType: "Right/Left open/laparoscopic/robotic partial nephrectomy",
            anesthesiaType: "General anesthesia",
            position: "Lateral decubitus or flank position",
            findings: "Renal mass measuring [__] cm located on the upper/mid/lower pole. The tumor was well-circumscribed and confined to the renal parenchyma without gross evidence of renal vein or perinephric extension.",
            operativeSteps: `1. Under general anesthesia, the patient was placed in lateral decubitus position and prepped and draped in sterile fashion.
2. A flank incision was made and the colon was reflected medially to expose the kidney.
3. The renal artery was identified and clamped to achieve temporary renal ischemia.
4. The tumor was excised with a rim of normal parenchyma using cold scissors or electrocautery.
5. Renorrhaphy was performed in two layers: inner layer to control collecting system and small vessels, outer cortical layer to approximate renal parenchyma.
6. The clamp was released and hemostasis was confirmed.
7. The specimen was sent for histopathology.
8. A drain was placed in the renal bed if indicated.
9. The incision was closed in layers.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "100–300 mL"
        };
        
        surgeryTemplates.radical_prostatectomy = {
            name: "Radical Prostatectomy",
            surgeryType: "Open/Laparoscopic/Robotic-assisted radical prostatectomy with bilateral pelvic lymph node dissection",
            anesthesiaType: "General anesthesia",
            position: "Supine/Low lithotomy with Trendelenburg position",
            findings: "Prostate of normal/enlarged size. No gross extracapsular extension. Seminal vesicles intact. No gross pelvic lymphadenopathy observed.",
            operativeSteps: `1. Under general anesthesia, the patient was positioned and prepped and draped in sterile fashion.
2. A lower midline incision was made or ports were inserted for robotic/laparoscopic access.
3. The bladder was mobilized and the endopelvic fascia was incised bilaterally.
4. The dorsal venous complex was ligated and divided.
5. Bladder neck was dissected and transected, preserving ureteric orifices.
6. The prostate was dissected posteriorly off the rectum.
7. Seminal vesicles and vas deferens were mobilized and divided.
8. The neurovascular bundles were preserved or resected based on oncologic criteria.
9. The apex of the prostate and urethra were divided with careful identification of urethral margins.
10. Pelvic lymph node dissection was performed bilaterally if indicated.
11. A watertight urethrovesical anastomosis was constructed using absorbable sutures.
12. A pelvic drain was placed. A 16 Fr Foley catheter was inserted and secured.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "250–500 mL"
        };

        surgeryTemplates.ureteric_reimplant = {
            name: "Ureteric Reimplantation",
            surgeryType: "Right/Left ureteric reimplantation (extravesical/Lich-Gregoir or intravesical/Cohen technique)",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Distal ureteric narrowing/refluxing ureter with/without associated hydronephrosis. Bladder capacity adequate. No gross bladder wall abnormalities.",
            operativeSteps: `1. Under general anesthesia, the patient was positioned supine, prepped, and draped in sterile fashion.
2. A Pfannenstiel or lower midline incision was made to access the bladder.
3. The bladder was exposed extraperitoneally.
4. The affected ureter was mobilized down to its distal segment, and the diseased portion was excised.
5. A detrusor tunnel was created on the bladder wall and the ureter was implanted into the bladder mucosa.
6. The detrusor muscle was re-approximated over the ureter to create an antireflux tunnel.
7. A double-J stent was placed in most cases.
8. The bladder was closed in two layers.
9. A pelvic drain and Foley catheter were inserted and secured.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "50–150 mL"
        };
        
        surgeryTemplates.pyeloplasty = {
            name: "Pyeloplasty",
            surgeryType: "Right/Left open/laparoscopic/robotic Anderson-Hynes dismembered pyeloplasty",
            anesthesiaType: "General anesthesia",
            position: "Flank or lateral decubitus position",
            findings: "Ureteropelvic junction obstruction with a dilated renal pelvis and narrow/angulated UPJ. No crossing vessel / Crossing vessel present and preserved.",
            operativeSteps: `1. Under general anesthesia, the patient was placed in lateral decubitus position and prepped and draped in sterile fashion.
2. A flank incision or ports were placed for laparoscopic/robotic approach.
3. The colon was reflected to expose the kidney and UPJ.
4. The renal pelvis and proximal ureter were dissected and mobilized.
5. The obstructed UPJ segment was excised.
6. The ureter was spatulated laterally and a tension-free anastomosis was fashioned with the renal pelvis (Anderson-Hynes technique).
7. A double-J stent was inserted antegrade into the ureter and bladder.
8. The anastomosis was completed using interrupted or continuous absorbable sutures.
9. Hemostasis was achieved. A drain was placed adjacent to the anastomosis.
10. The incision/port sites were closed in layers.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "50–150 mL"
        };
        
        surgeryTemplates.cystolitholapaxy = {
            name: "Cystolitholapaxy",
            surgeryType: "Transurethral cystolitholapaxy for vesical stone(s)",
            anesthesiaType: "Spinal anesthesia",
            position: "Lithotomy position",
            findings: "One/multiple bladder stone(s) visualized, measuring approximately [__] cm. Bladder mucosa intact. Ureteric orifices visualized and preserved. No associated bladder tumor or foreign body seen.",
            operativeSteps: `1. The patient was positioned in lithotomy under spinal anesthesia, prepped, and draped in sterile fashion.
2. A cystoscope was introduced into the bladder, and stone(s) were identified and assessed for size, number, and mobility.
3. A lithotripsy device (pneumatic/ultrasonic/laser) was introduced via a working sheath.
4. The stone(s) were fragmented under direct vision.
5. Stone fragments were irrigated and removed with a stone evacuator (e.g., Ellik evacuator).
6. Complete clearance of the bladder was confirmed cystoscopically.
7. Ureteric orifices were inspected.
8. A 20/22 Fr Foley catheter was inserted, and irrigation was initiated if needed.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<50 mL)"
        };
        
        surgeryTemplates.radical_orchiectomy = {
            name: "Radical Inguinal Orchiectomy",
            surgeryType: "Right/Left radical inguinal orchiectomy",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Testis appeared enlarged/firm with no evidence of scrotal wall involvement. Spermatic cord structures were thickened/normal. No gross invasion of surrounding tissue.",
            operativeSteps: `1. The patient was positioned supine under general anesthesia and prepped and draped in sterile fashion.
2. A groin incision was made along the inguinal canal.
3. The external oblique aponeurosis was opened to access the inguinal canal.
4. The spermatic cord was isolated at the internal ring, clamped, divided, and ligated using high ligation technique.
5. The testis, epididymis, and spermatic cord were delivered en bloc.
6. Hemostasis was achieved.
7. The inguinal canal and skin were closed in layers.
8. The specimen was sent for histopathology.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "<50 mL"
        };
        
        surgeryTemplates.simple_orchiectomy = {
            name: "Simple Orchiectomy",
            surgeryType: "Right/Left simple orchiectomy",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Testis was atrophic/painful/nonfunctional. Spermatic cord appeared normal. No associated masses or abnormal findings.",
            operativeSteps: `1. The patient was placed supine under anesthesia, prepped, and draped in sterile fashion.
2. A transverse scrotal incision was made over the affected testis.
3. The testis and epididymis were delivered through the incision.
4. The spermatic cord was clamped and ligated at the level of the external ring.
5. The testis and attached cord were excised.
6. Hemostasis was confirmed, and the scrotal wound was closed in layers using absorbable sutures.
7. Dressing was applied.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<20–30 mL)"
        };
        
        surgeryTemplates.scrotal_exploration = {
            name: "Scrotal Exploration",
            surgeryType: "Right/Left scrotal exploration ± detorsion, orchiopexy, or orchiectomy",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Testis was found to be twisted by [__] degrees / no torsion noted. Testis appeared viable / congested / ischemic. Tunica vaginalis contained clear fluid / hemorrhagic fluid. Spermatic cord was twisted / intact.",
            operativeSteps: `1. Under general anesthesia, the patient was placed in supine position, prepped, and draped in sterile fashion.
2. A transverse midline or hemiscrotal incision was made over the affected side.
3. The tunica vaginalis was opened, and the testis was delivered into the wound.
4. The testis and spermatic cord were inspected.
5. If torsion was found: the cord was untwisted, and warm sponges applied for 10–15 minutes to assess viability.
6. If viable: orchiopexy was performed using non-absorbable sutures.
7. If non-viable: orchiectomy was performed and the cord ligated.
8. Contralateral orchiopexy was performed through a separate incision if indicated.
9. Hemostasis was achieved.
10. The dartos and skin were closed in layers using absorbable sutures.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<30 mL)"
        };
        
        surgeryTemplates.suprapubic_cystostomy = {
            name: "Suprapubic Cystostomy",
            surgeryType: "Suprapubic cystostomy for urinary diversion",
            anesthesiaType: "Local anesthesia ± sedation",
            position: "Supine position",
            findings: "Distended bladder identified with adequate capacity. No evidence of bladder tumor, clot retention, or bladder wall abnormality on preoperative evaluation.",
            operativeSteps: `1. Under local anesthesia, the patient was positioned supine, prepped, and draped in sterile fashion.
2. The bladder was filled via a urethral catheter or guided by ultrasound.
3. A 2–3 cm transverse infraumbilical incision was made above the pubic symphysis.
4. Dissection was carried through subcutaneous tissues and rectus sheath to the bladder dome.
5. The bladder was punctured using a trocar/cystostomy set under direct palpation or ultrasound guidance.
6. Urine was aspirated to confirm entry, and a Foley catheter (14–16 Fr) was introduced through the tract into the bladder.
7. The catheter balloon was inflated and traction applied to secure the catheter.
8. The skin was closed around the tube and secured with sutures or adhesive dressing.
9. Urine drained freely, and catheter patency was confirmed.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<10 mL)"
        };
        
        surgeryTemplates.penile_prosthesis = {
            name: "Penile Prosthesis Implantation",
            surgeryType: "Insertion of inflatable/malleable penile prosthesis",
            anesthesiaType: "General anesthesia",
            position: "Supine position",
            findings: "Corpora cavernosa were of adequate length and dilatable. No fibrosis or significant corporal scarring. Urethra intact. No signs of infection or abnormal anatomy.",
            operativeSteps: `1. Under anesthesia, the patient was positioned supine and prepped and draped in sterile fashion with a wide perineal/genital field.
2. A penile/scrotal or infrapubic incision was made depending on surgeon's approach.
3. Corporotomies were made bilaterally over the corpora cavernosa.
4. Corporal bodies were dilated proximally and distally with Hegar dilators.
5. Measurement was taken, and appropriate-sized prosthesis cylinders were inserted bilaterally.
6. If inflatable type: Pump was placed in the scrotum, Reservoir placed in the retropubic space, Tubing was connected and tested for function and leaks.
7. Corporotomies were closed with absorbable sutures.
8. Wound was irrigated with antibiotic solution, and incision closed in layers.
9. A compressive dressing and Foley catheter were applied.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "50–100 mL"
        };
        
        surgeryTemplates.bladder_neck_incision = {
            name: "Bladder Neck Incision (BNI)",
            surgeryType: "Transurethral bladder neck incision for bladder outlet obstruction",
            anesthesiaType: "Spinal anesthesia",
            position: "Lithotomy position",
            findings: "Narrow bladder neck with high bladder neck elevation and no significant prostatic enlargement. Ureteric orifices were clearly identified and distant from the incision site.",
            operativeSteps: `1. Under anesthesia, the patient was positioned in lithotomy and prepped and draped in sterile fashion.
2. A cystoscopic evaluation was performed, confirming a tight bladder neck with no obstructing median or lateral prostate lobes.
3. A resectoscope with cold knife or Collins knife was introduced.
4. A deep incision was made at the 5 o'clock and 7 o'clock positions through the bladder neck muscle fibers until perivesical fat was seen.
5. Hemostasis was achieved with coagulation as needed.
6. The bladder was inspected to ensure no injury to the trigone or ureteric orifices.
7. A 16–18 Fr Foley catheter was inserted and bladder irrigation was initiated if necessary.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<30 mL)"
        };
        
        surgeryTemplates.urethroplasty = {
            name: "Urethroplasty",
            surgeryType: "Excision and primary anastomosis urethroplasty / Buccal mucosal graft urethroplasty for anterior urethral stricture",
            anesthesiaType: "General anesthesia",
            position: "Lithotomy position",
            findings: "Segmental bulbar urethral stricture measuring [__] cm. Healthy urethral mucosa proximal and distal to the stricture. Corpus spongiosum intact.",
            operativeSteps: `1. The patient was placed in the lithotomy position and prepped and draped in sterile fashion.
2. A midline perineal incision was made and the bulbar urethra was dissected and fully mobilized.
3. The strictured segment was identified and excised.
4. If length <2 cm: The healthy ends of the urethra were spatulated and a tension-free end-to-end anastomosis was performed with interrupted absorbable sutures.
5. If length >2–3 cm: A buccal mucosal graft was harvested and quilted onto the corpora, and urethrotomy edges were sutured to the graft.
6. A 16 Fr silicone catheter was passed across the repair.
7. Hemostasis was achieved and a drain placed in the perineal wound.
8. The wound was closed in layers.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "50–150 mL"
        };
        
        surgeryTemplates.meatotomy = {
            name: "Meatotomy / Meatoplasty",
            surgeryType: "Meatotomy / Meatoplasty for meatal stenosis",
            anesthesiaType: "Local anesthesia with/without sedation",
            position: "Supine position",
            findings: "Tight stenosis of the external urethral meatus with pinpoint orifice. No other urethral abnormalities noted on retrograde calibration or cystoscopy.",
            operativeSteps: `1. The patient was placed in the supine position and prepped and draped in sterile fashion.
2. Local anesthesia was infiltrated around the glans and urethral meatus.
3. A ventral incision was made at the 6 o'clock position through the stenosed segment using a fine-tipped blade.
4. The incision was extended until a wide and adequate meatus was achieved.
5. For meatotomy: the incision was left open or sutured laterally if needed.
6. For meatoplasty: Triangular skin flaps were created and advanced to widen the neomeatus with interrupted absorbable sutures.
7. A catheter (8–12 Fr) was left in situ for 24–72 hours.
8. Hemostasis was ensured and dressing applied.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<10 mL)"
        };
        
        surgeryTemplates.hypospadias = {
            name: "Hypospadias Repair (TIP Urethroplasty)",
            surgeryType: "Hypospadias repair with tubularized incised plate (TIP) urethroplasty ± chordee correction ± circumcision",
            anesthesiaType: "General anesthesia with caudal block",
            position: "Supine position",
            findings: "[Distal/midshaft/proximal] hypospadias with ventral meatal opening. Urethral plate adequate for tubularization. Mild/moderate chordee [present/absent]. Penile torsion [present/absent].",
            operativeSteps: `1. Under general anesthesia, the patient was positioned supine and prepped and draped in sterile fashion.
2. Stay sutures were placed on the glans for traction.
3. A circumferential incision was made proximal to the coronal sulcus, and penile skin was degloved down to the base.
4. Artificial erection test was performed to assess chordee.
5. If present, chordee was corrected by dorsal plication or urethral plate mobilization.
6. The urethral plate was incised longitudinally in the midline.
7. A neourethra was tubularized over an 8–10 Fr stent using interrupted or continuous absorbable sutures.
8. A second vascularized layer (dartos flap) was rotated from dorsal/preputial skin and placed over the neourethra.
9. Glans wings were mobilized and closed in the midline over the neourethra.
10. Penile skin was re-approximated, and circumcision completed if indicated.
11. A urethral stent or catheter was left in situ and secured.
12. Compressive dressing was applied.`,
            complications: "None encountered intraoperatively",
            bloodLoss: "Minimal (<10–20 mL)"
        };
        
        // DOM elements for operative notes
        const operativeNoteView = document.getElementById('operativeNoteView');
        const viewOperativeNotesView = document.getElementById('viewOperativeNotesView');
        const operativeNoteForm = document.getElementById('operativeNoteForm');
        const cancelOperativeNote = document.getElementById('cancelOperativeNote');
        const operativeNotesList = document.getElementById('operativeNotesList');
        const backFromOperativeNotes = document.getElementById('backFromOperativeNotes');
        
        // Template button handlers
        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('template-btn')) {
                const templateKey = e.target.getAttribute('data-template');
                const template = surgeryTemplates[templateKey];
                
                if (template) {
                    // Remove active class from all template buttons
                    document.querySelectorAll('.template-btn').forEach(btn => {
                        btn.classList.remove('active');
                    });
                    
                    // Add active class to clicked button
                    e.target.classList.add('active');
                    
                    // Fill form with template data
                    document.getElementById('surgeryType').value = template.surgeryType;
                    document.getElementById('anesthesiaType').value = template.anesthesiaType;
                    document.getElementById('position').value = template.position;
                    document.getElementById('findings').value = template.findings;
                    document.getElementById('operativeSteps').value = template.operativeSteps;
                    document.getElementById('complications').value = template.complications;
                    document.getElementById('bloodLoss').value = template.bloodLoss;
                    
                    // Set today's date and current user as surgeon
                    document.getElementById('surgeryDate').valueAsDate = new Date();
                    document.getElementById('surgeon').value = userEmail.textContent.split('@')[0].replace('.', ' ');
                }
            }
        });
        
        // Operative note form handler
        operativeNoteForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            try {
                const operativeNoteData = {
                    surgeryType: document.getElementById('surgeryType').value,
                    surgeryDate: document.getElementById('surgeryDate').value,
                    surgeryTime: document.getElementById('surgeryTime').value,
                    surgeon: document.getElementById('surgeon').value,
                    assistants: document.getElementById('assistants').value,
                    anesthesiaType: document.getElementById('anesthesiaType').value,
                    position: document.getElementById('position').value,
                    findings: document.getElementById('findings').value,
                    operativeSteps: document.getElementById('operativeSteps').value,
                    complications: document.getElementById('complications').value,
                    bloodLoss: document.getElementById('bloodLoss').value,
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    createdBy: currentUser.uid,
                    createdByName: userEmail.textContent,
                    patientId: currentPatientId
                };
                
                if (editingOperativeNote && currentOperativeNoteId) {
                    // Update existing operative note
                    await db.collection('patients').doc(currentPatientId).collection('operativeNotes').doc(currentOperativeNoteId).update({
                          ...operativeNoteData,
                   updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
                   updatedBy: currentUser.uid,
                   updatedByName: userEmail.textContent
                   });
                   showMessage('operativeNoteSuccess', 'Operative note updated successfully!', true);
            } else {
                // Add new operative note
                await db.collection('patients').doc(currentPatientId).collection('operativeNotes').add(operativeNoteData);
                showMessage('operativeNoteSuccess', 'Operative note saved successfully!', true);
            }

                // Update patient status to post-op
                await db.collection('patients').doc(currentPatientId).update({
                       status: 'post-op',
                       updatedAt: firebase.firestore.FieldValue.serverTimestamp()
                });

                // Update current patient data
                currentPatientData.status = 'post-op';
                
                operativeNoteForm.reset();
                
                setTimeout(() => {
                    showView('patientDetail');
                    displayPatientDetail(currentPatientData);
                }, 2000);
                
            } catch (error) {
                console.error('Error saving operative note:', error);
                showMessage('operativeNoteError', 'Error saving operative note. Please try again.');
            }
        });
        
        // Navigation handlers
        cancelOperativeNote.addEventListener('click', () => {
            showView('patientDetail');
        });
        
        backFromOperativeNotes.addEventListener('click', () => {
            showView('patientDetail');
        });

        // View Daily Logs button handler
        document.getElementById('viewDailyLogsBtn').addEventListener('click', () => {
            showView('dailyLogs');
            loadDailyLogs(currentPatientId);
        });
        
        // Update the "Add Operative Note" button handler
        document.getElementById('addOperativeNoteBtn').addEventListener('click', () => {
            showView('operativeNote');
            operativeNoteForm.reset();
            document.getElementById('surgeryDate').valueAsDate = new Date();
            document.getElementById('surgeon').value = userEmail.textContent.split('@')[0].replace('.', ' ');
            
            // Remove active state from all template buttons
            document.querySelectorAll('.template-btn').forEach(btn => {
                btn.classList.remove('active');
            });
        });

        // Daily Logs functionality
        let currentDailyLogId = null;
        let editingDailyLog = false;
        let allDailyLogs = [];
        
        // DOM elements for daily logs
        const dailyLogsView = document.getElementById('dailyLogsView');
        const addDailyLogView = document.getElementById('addDailyLogView');
        const dailyLogForm = document.getElementById('dailyLogForm');
        const dailyLogsList = document.getElementById('dailyLogsList');
        const addDailyLogBtn = document.getElementById('addDailyLogBtn');
        const backFromDailyLogs = document.getElementById('backFromDailyLogs');
        const cancelDailyLog = document.getElementById('cancelDailyLog');
        const showTableView = document.getElementById('showTableView');
        const showChartView = document.getElementById('showChartView');
        const logsTableView = document.getElementById('logsTableView');
        const logsChartView = document.getElementById('logsChartView');
        
        // Update showView function to handle daily logs
        const originalShowView = showView;
        showView = function(viewName) {
            // Hide all views including daily logs
            const views = [
                'patientsView', 'addPatientView', 'patientDetailView', 'editPatientView',
                'operativeNoteView', 'viewOperativeNotesView', 'dailyLogsView', 'addDailyLogView'
            ];
            
            views.forEach(viewId => {
                const element = document.getElementById(viewId);
                if (element) element.classList.add('hidden');
            });
    
            // Reset button states
            const showPatientsBtn = document.getElementById('showPatientsBtn');
            const showAddPatientBtn = document.getElementById('showAddPatientBtn');
            if (showPatientsBtn) showPatientsBtn.classList.remove('btn-primary');
            if (showAddPatientBtn) showAddPatientBtn.classList.remove('btn-primary');
    
            // Show requested view
            if (viewName === 'patients') {
                const patientsView = document.getElementById('patientsView');
                if (patientsView) patientsView.classList.remove('hidden');
                if (showPatientsBtn) showPatientsBtn.classList.add('btn-primary');
            } else if (viewName === 'addPatient') {
                const addPatientView = document.getElementById('addPatientView');
                if (addPatientView) addPatientView.classList.remove('hidden');
                if (showAddPatientBtn) showAddPatientBtn.classList.add('btn-primary');
            } else if (viewName === 'patientDetail') {
                const patientDetailView = document.getElementById('patientDetailView');
                if (patientDetailView) patientDetailView.classList.remove('hidden');
            } else if (viewName === 'editPatient') {
                const editPatientView = document.getElementById('editPatientView');
                if (editPatientView) editPatientView.classList.remove('hidden');
            } else if (viewName === 'operativeNote') {
                const operativeNoteView = document.getElementById('operativeNoteView');
                if (operativeNoteView) operativeNoteView.classList.remove('hidden');
            } else if (viewName === 'viewOperativeNotes') {
                const viewOperativeNotesView = document.getElementById('viewOperativeNotesView');
                if (viewOperativeNotesView) viewOperativeNotesView.classList.remove('hidden');
            } else if (viewName === 'dailyLogs') {
                const dailyLogsView = document.getElementById('dailyLogsView');
                if (dailyLogsView) dailyLogsView.classList.remove('hidden');
            } else if (viewName === 'addDailyLog') {
                const addDailyLogView = document.getElementById('addDailyLogView');
                if (addDailyLogView) addDailyLogView.classList.remove('hidden');
            }
        };
        
        // Check vital signs for abnormal values
        function checkVitalNormal(vital, value) {
            if (!value || value === '') return '';
            
            const numValue = parseFloat(value);
            
            switch(vital) {
                case 'systolic':
                    return (numValue >= 90 && numValue <= 140) ? 'normal' : 'abnormal';
                case 'diastolic':
                    return (numValue >= 60 && numValue <= 90) ? 'normal' : 'abnormal';
                case 'heartRate':
                    return (numValue >= 60 && numValue <= 100) ? 'normal' : 'abnormal';
                case 'temperature':
                    return (numValue >= 36.1 && numValue <= 37.2) ? 'normal' : 'abnormal';
                case 'respiratoryRate':
                    return (numValue >= 12 && numValue <= 20) ? 'normal' : 'abnormal';
                case 'oxygenSaturation':
                    return (numValue >= 95) ? 'normal' : 'abnormal';
                case 'painScore':
                    return (numValue <= 3) ? 'normal' : 'abnormal';
                default:
                    return '';
            }
        }
        
        // Load and display daily logs
        async function loadDailyLogs(patientId) {
            try {
                const container = document.getElementById('dailyLogsList');
                container.innerHTML = '<div class="loading"><div class="spinner"></div><p>Loading daily logs...</p></div>';
                
                const snapshot = await db.collection('patients').doc(patientId).collection('dailyLogs').orderBy('createdAt', 'desc').get();
                
                allDailyLogs = [];
                snapshot.forEach(doc => {
                    allDailyLogs.push({
                        id: doc.id,
                        ...doc.data()
                    });
                });
                
                displayDailyLogs(allDailyLogs);
                
            } catch (error) {
                console.error('Error loading daily logs:', error);
                document.getElementById('dailyLogsList').innerHTML = 
                    '<p style="color: #f44336; text-align: center;">Error loading daily logs.</p>';
            }
        }

        // Display daily logs
        function displayDailyLogs(logs) {
            const container = document.getElementById('dailyLogsList');
            
            if (logs.length === 0) {
                container.innerHTML = `
                    <div class="no-daily-logs">
                        <h3>📊 No Daily Logs Yet</h3>
                        <p>Start tracking this patient's post-operative progress by adding the first daily log.</p>
                        <button class="btn btn-primary" onclick="showAddDailyLogForm()">➕ Add First Log</button>
                    </div>
                `;
                return;
            }
            
            const logsHtml = logs.map(log => {
                const date = log.createdAt ? log.createdAt.toDate().toLocaleDateString() : log.logDate || 'Unknown date';
                const time = log.createdAt ? log.createdAt.toDate().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) : log.logTime || '';
                
                return `
                    <div class="daily-log-card">
                        <div class="daily-log-header">
                            <h3>📅 ${date} ${time}</h3>
                            <div class="daily-log-meta">
                                <span>Day ${log.dayPostOp || '?'} Post-Op</span>
                                <span>By: ${log.createdByName || 'Unknown'}</span>
                            </div>
                        </div>
                        
                        <div class="vitals-grid">
                            <div class="vital-item">
                                <div class="vital-label">BP</div>
                                <div class="vital-value ${log.systolicBP ? checkVitalNormal('systolic', log.systolicBP) : ''}">${log.systolicBP || '-'}/${log.diastolicBP || '-'}</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">HR</div>
                                <div class="vital-value ${log.heartRate ? checkVitalNormal('heartRate', log.heartRate) : ''}">${log.heartRate || '-'} bpm</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">Temp</div>
                                <div class="vital-value ${log.temperature ? checkVitalNormal('temperature', log.temperature) : ''}">${log.temperature || '-'}°C</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">RR</div>
                                <div class="vital-value ${log.respiratoryRate ? checkVitalNormal('respiratoryRate', log.respiratoryRate) : ''}">${log.respiratoryRate || '-'}/min</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">O2 Sat</div>
                                <div class="vital-value ${log.oxygenSaturation ? checkVitalNormal('oxygenSaturation', log.oxygenSaturation) : ''}">${log.oxygenSaturation || '-'}%</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">Pain</div>
                                <div class="vital-value ${log.painScore ? checkVitalNormal('painScore', log.painScore) : ''}">${log.painScore || '-'}/10</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">Urine</div>
                                <div class="vital-value">${log.urineOutput || '-'} mL</div>
                            </div>
                            <div class="vital-item">
                                <div class="vital-label">Drain</div>
                                <div class="vital-value">${log.drainOutput || '-'} mL</div>
                            </div>
                        </div>
                        
                        <div class="daily-log-content">
                            ${log.progressNote ? `
                                <div class="log-section">
                                    <h4>📝 Progress Note</h4>
                                    <p>${log.progressNote}</p>
                                </div>
                            ` : ''}
                            
                            ${log.medicationsGiven ? `
                                <div class="log-section">
                                    <h4>💊 Medications Given</h4>
                                    <p>${log.medicationsGiven}</p>
                                </div>
                            ` : ''}
                            
                            ${log.clinicalNotes ? `
                                <div class="log-section">
                                    <h4>🩺 Additional Notes</h4>
                                    <p>${log.clinicalNotes}</p>
                                </div>
                            ` : ''}
                        </div>
                        
                        <div class="daily-log-actions">
                            <button class="btn btn-sm btn-secondary" onclick="editDailyLog('${log.id}', '${currentPatientId}')">
                                ✏️ Edit
                            </button>
                            <button class="btn btn-sm btn-danger" onclick="deleteDailyLog('${log.id}', '${currentPatientId}')">
                                🗑️ Delete
                            </button>
                        </div>
                    </div>
                `;
            }).join('');
            
            container.innerHTML = logsHtml;
        }
        
        // Show add daily log form
        function showAddDailyLogForm() {
            showView('addDailyLog');
            dailyLogForm.reset();
            
            // Set today's date and current time
            document.getElementById('logDate').valueAsDate = new Date();
            document.getElementById('logTime').value = new Date().toTimeString().slice(0,5);
            
            // Calculate day post-op if we have surgery date
            if (currentPatientData && currentPatientData.admissionDate) {
                const admissionDate = new Date(currentPatientData.admissionDate);
                const today = new Date();
                const daysDiff = Math.ceil((today - admissionDate) / (1000 * 60 * 60 * 24));
                document.getElementById('dayPostOp').value = daysDiff > 0 ? daysDiff : 1;
            }
            
            // Reset editing state
            editingDailyLog = false;
            currentDailyLogId = null;
            
            // Update form title
            document.querySelector('#addDailyLogView h2').textContent = '➕ Add Daily Log';
            document.querySelector('#dailyLogForm button[type="submit"]').textContent = 'Save Daily Log';
        }
        
        // Edit daily log
        async function editDailyLog(logId, patientId) {
            try {
                currentDailyLogId = logId;
                editingDailyLog = true;
                
                // Load the daily log data
                const doc = await db.collection('patients').doc(patientId).collection('dailyLogs').doc(logId).get();
                
                if (doc.exists) {
                    const log = doc.data();
                    
                    // Switch to daily log form
                    showView('addDailyLog');
                    
                    // Fill form with existing data
                    document.getElementById('logDate').value = log.logDate || '';
                    document.getElementById('logTime').value = log.logTime || '';
                    document.getElementById('dayPostOp').value = log.dayPostOp || '';
                    document.getElementById('systolicBP').value = log.systolicBP || '';
                    document.getElementById('diastolicBP').value = log.diastolicBP || '';
                    document.getElementById('heartRate').value = log.heartRate || '';
                    document.getElementById('temperature').value = log.temperature || '';
                    document.getElementById('respiratoryRate').value = log.respiratoryRate || '';
                    document.getElementById('oxygenSaturation').value = log.oxygenSaturation || '';
                    document.getElementById('painScore').value = log.painScore || '';
                    document.getElementById('urineOutput').value = log.urineOutput || '';
                    document.getElementById('drainOutput').value = log.drainOutput || '';
                    document.getElementById('progressNote').value = log.progressNote || '';
                    document.getElementById('medicationsGiven').value = log.medicationsGiven || '';
                    document.getElementById('clinicalNotes').value = log.clinicalNotes || '';
                    
                    // Update form title and button text
                    document.querySelector('#addDailyLogView h2').textContent = '✏️ Edit Daily Log';
                    document.querySelector('#dailyLogForm button[type="submit"]').textContent = 'Update Daily Log';
                    
                } else {
                    alert('Daily log not found');
                }
                
            } catch (error) {
                console.error('Error loading daily log for editing:', error);
                alert('Error loading daily log');
            }
        }
        
        // Delete daily log
        async function deleteDailyLog(logId, patientId) {
            if (confirm('Are you sure you want to delete this daily log? This action cannot be undone.')) {
                try {
                    await db.collection('patients').doc(patientId).collection('dailyLogs').doc(logId).delete();
                    
                    // Reload daily logs to refresh the display
                    loadDailyLogs(patientId);
                    
                    alert('Daily log deleted successfully');
                    
                } catch (error) {
                    console.error('Error deleting daily log:', error);
                    alert('Error deleting daily log');
                }
            }
        }

        // Daily log form handler
        dailyLogForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            try {
                const dailyLogData = {
                    logDate: document.getElementById('logDate').value,
                    logTime: document.getElementById('logTime').value,
                    dayPostOp: parseInt(document.getElementById('dayPostOp').value) || null,
                    systolicBP: parseInt(document.getElementById('systolicBP').value) || null,
                    diastolicBP: parseInt(document.getElementById('diastolicBP').value) || null,
                    heartRate: parseInt(document.getElementById('heartRate').value) || null,
                    temperature: parseFloat(document.getElementById('temperature').value) || null,
                    respiratoryRate: parseInt(document.getElementById('respiratoryRate').value) || null,
                    oxygenSaturation: parseInt(document.getElementById('oxygenSaturation').value) || null,
                    painScore: parseInt(document.getElementById('painScore').value) || null,
                    urineOutput: parseInt(document.getElementById('urineOutput').value) || null,
                    drainOutput: parseInt(document.getElementById('drainOutput').value) || null,
                    progressNote: document.getElementById('progressNote').value,
                    medicationsGiven: document.getElementById('medicationsGiven').value,
                    clinicalNotes: document.getElementById('clinicalNotes').value,
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    createdBy: currentUser.uid,
                    createdByName: userEmail.textContent,
                    patientId: currentPatientId
                };
                
                if (editingDailyLog && currentDailyLogId) {
                    // Update existing daily log
                    await db.collection('patients').doc(currentPatientId).collection('dailyLogs').doc(currentDailyLogId).update({
                        ...dailyLogData,
                        updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
                        updatedBy: currentUser.uid,
                        updatedByName: userEmail.textContent
                    });
                    showMessage('dailyLogSuccess', 'Daily log updated successfully!', true);
                } else {
                    // Add new daily log
                    await db.collection('patients').doc(currentPatientId).collection('dailyLogs').add(dailyLogData);
                    showMessage('dailyLogSuccess', 'Daily log saved successfully!', true);
                }
                
                dailyLogForm.reset();
                
                setTimeout(() => {
                    showView('dailyLogs');
                    loadDailyLogs(currentPatientId);
                }, 2000);
                
            } catch (error) {
                console.error('Error saving daily log:', error);
                showMessage('dailyLogError', 'Error saving daily log. Please try again.');
            }
        });
        
        // View toggle handlers
        showTableView.addEventListener('click', () => {
            logsTableView.classList.remove('hidden');
            logsChartView.classList.add('hidden');
            showTableView.classList.add('btn-primary');
            showTableView.classList.remove('btn-secondary');
            showChartView.classList.remove('btn-primary');
            showChartView.classList.add('btn-secondary');
        });
        
        showChartView.addEventListener('click', () => {
            logsTableView.classList.add('hidden');
            logsChartView.classList.remove('hidden');
            showChartView.classList.add('btn-primary');
            showChartView.classList.remove('btn-secondary');
            showTableView.classList.remove('btn-primary');
            showTableView.classList.add('btn-secondary');
            
            // Generate chart
            generateVitalsChart();
        });
        
        // Simple chart generation using HTML/CSS (no external libraries needed)
        function generateVitalsChart() {
            const chartContainer = document.getElementById('vitalsChart');
            
            if (allDailyLogs.length === 0) {
                chartContainer.innerHTML = '<p style="text-align: center; color: #666; padding: 40px;">No data available for chart visualization.</p>';
                return;
            }
            
            // Sort logs by date
            const sortedLogs = [...allDailyLogs].sort((a, b) => {
                const dateA = new Date(a.logDate || a.createdAt?.toDate());
                const dateB = new Date(b.logDate || b.createdAt?.toDate());
                return dateA - dateB;
            });
            
            // Create simple table-based chart
            const chartData = sortedLogs.map(log => {
                const date = log.logDate || (log.createdAt ? log.createdAt.toDate().toLocaleDateString() : 'Unknown');
                return {
                    date: date,
                    systolic: log.systolicBP || 0,
                    diastolic: log.diastolicBP || 0,
                    heartRate: log.heartRate || 0,
                    temperature: log.temperature || 0,
                    pain: log.painScore || 0
                };
            });
            
            let chartHtml = `
                <div style="overflow-x: auto;">
                    <table style="width: 100%; border-collapse: collapse; font-size: 14px;">
                        <thead>
                            <tr style="background: #f5f5f5;">
                                <th style="padding: 10px; border: 1px solid #ddd; text-align: left;">Date</th>
                                <th style="padding: 10px; border: 1px solid #ddd; text-align: center;">Systolic BP</th>
                                <th style="padding: 10px; border: 1px solid #ddd; text-align: center;">Diastolic BP</th>
                                <th style="padding: 10px; border: 1px solid #ddd; text-align: center;">Heart Rate</th>
                                <th style="padding: 10px; border: 1px solid #ddd; text-align: center;">Temperature</th>
                                <th style="padding: 10px; border: 1px solid #ddd; text-align: center;">Pain Score</th>
                            </tr>
                        </thead>
                        <tbody>
            `;
            
            chartData.forEach(data => {
                chartHtml += `
                    <tr>
                        <td style="padding: 8px; border: 1px solid #ddd;">${data.date}</td>
                        <td style="padding: 8px; border: 1px solid #ddd; text-align: center;">${data.systolic || '-'}</td>
                        <td style="padding: 8px; border: 1px solid #ddd; text-align: center;">${data.diastolic || '-'}</td>
                        <td style="padding: 8px; border: 1px solid #ddd; text-align: center;">${data.heartRate || '-'}</td>
                        <td style="padding: 8px; border: 1px solid #ddd; text-align: center;">${data.temperature || '-'}</td>
                        <td style="padding: 8px; border: 1px solid #ddd; text-align: center;">${data.pain || '-'}</td>
                    </tr>
                `;
            });
            
            chartHtml += `
                        </tbody>
                    </table>
                </div>
                <p style="text-align: center; color: #666; margin-top: 15px; font-size: 12px;">
                    📊 Trends: Compare values across dates to monitor patient recovery
                </p>
            `;
            
            chartContainer.innerHTML = chartHtml;
        }
        
        // Navigation event handlers
        addDailyLogBtn.addEventListener('click', showAddDailyLogForm);
        backFromDailyLogs.addEventListener('click', () => showView('patientDetail'));
        cancelDailyLog.addEventListener('click', () => showView('dailyLogs'));
        
        // Initialize app
        showScreen('loading');
    </script>
</body>
</html>
