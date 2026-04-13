<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EMIS Pro | Comprehensive Student Management System - AEP/ASR/Conventional | GPA · Rank · Attendance · Dropout</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%); min-height: 100vh; padding: 20px; }
        .dashboard-container { max-width: 1600px; margin: 0 auto; }
        .header { background: rgba(255,255,255,0.95); border-radius: 24px; padding: 20px 30px; margin-bottom: 25px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }
        .logo h1 { font-size: 1.6rem; background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .logo p { color: #64748b; font-size: 0.75rem; }
        .stats-badge { display: flex; gap: 12px; flex-wrap: wrap; }
        .badge { background: #f8fafc; padding: 8px 15px; border-radius: 14px; text-align: center; border: 1px solid #e2e8f0; }
        .badge .number { font-size: 1.3rem; font-weight: bold; color: #2563eb; }
        .badge .label { font-size: 0.65rem; color: #64748b; }
        .nav-tabs { display: flex; gap: 5px; background: rgba(255,255,255,0.95); border-radius: 50px; padding: 5px 15px; margin-bottom: 25px; flex-wrap: wrap; overflow-x: auto; }
        .nav-btn { padding: 8px 18px; border: none; background: transparent; font-size: 0.8rem; font-weight: 600; cursor: pointer; border-radius: 40px; transition: all 0.3s; color: #475569; white-space: nowrap; }
        .nav-btn:hover { background: #e2e8f0; }
        .nav-btn.active { background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); color: white; }
        .section { display: none; animation: fadeIn 0.3s ease; }
        .section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .card { background: white; border-radius: 20px; padding: 20px; margin-bottom: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); border: 1px solid #e2e8f0; }
        .card-title { font-size: 1.1rem; font-weight: 700; margin-bottom: 15px; display: flex; align-items: center; gap: 8px; color: #1e293b; border-left: 4px solid #2563eb; padding-left: 12px; }
        .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(450px, 1fr)); gap: 20px; }
        .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; }
        .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 15px; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: 600; font-size: 0.75rem; color: #334155; }
        input, select, textarea { width: 100%; padding: 8px 12px; border: 2px solid #e2e8f0; border-radius: 10px; font-size: 0.8rem; font-family: inherit; }
        input:focus, select:focus { outline: none; border-color: #2563eb; }
        .btn { padding: 8px 16px; border: none; border-radius: 10px; font-weight: 600; cursor: pointer; transition: all 0.2s; font-size: 0.75rem; display: inline-flex; align-items: center; gap: 6px; }
        .btn-primary { background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); color: white; }
        .btn-primary:hover { transform: translateY(-2px); }
        .btn-secondary { background: #f1f5f9; color: #334155; border: 1px solid #e2e8f0; }
        .btn-danger { background: #fee2e2; color: #dc2626; }
        .btn-success { background: #d1fae5; color: #059669; }
        .btn-warning { background: #fed7aa; color: #ea580c; }
        .btn-sm { padding: 4px 8px; font-size: 0.65rem; }
        .data-table { width: 100%; border-collapse: collapse; font-size: 0.7rem; overflow-x: auto; display: block; }
        .data-table th, .data-table td { padding: 8px 6px; text-align: left; border-bottom: 1px solid #e2e8f0; }
        .data-table th { background: #f8fafc; font-weight: 700; color: #1e293b; position: sticky; top: 0; }
        .data-table tr:hover { background: #f8fafc; }
        .progress-bar-container { background: #e2e8f0; border-radius: 20px; overflow: hidden; height: 6px; }
        .progress-bar { height: 100%; border-radius: 20px; transition: width 0.5s ease; }
        .progress-high { background: #10b981; }
        .progress-medium { background: #f59e0b; }
        .progress-low { background: #ef4444; }
        .alert { padding: 10px 14px; border-radius: 10px; margin-bottom: 15px; font-size: 0.75rem; }
        .alert-info { background: #dbeafe; color: #1e40af; border-left: 4px solid #2563eb; }
        .alert-success { background: #d1fae5; color: #065f46; border-left: 4px solid #10b981; }
        .alert-warning { background: #fed7aa; color: #92400e; border-left: 4px solid #f59e0b; }
        .formula-box { background: #fef3c7; padding: 8px; border-radius: 8px; font-family: monospace; font-size: 0.7rem; margin-top: 8px; }
        .footer { text-align: center; padding: 20px; color: rgba(255,255,255,0.5); font-size: 0.7rem; }
        .rank-1 { background: #fef3c7; font-weight: bold; }
        .rank-2 { background: #e0e7ff; }
        .rank-3 { background: #d1fae5; }
        .attendance-present { color: #10b981; font-weight: bold; }
        .attendance-absent { color: #ef4444; }
        .dropdown-highlight { background: #f8fafc; padding: 5px; border-radius: 8px; }
        @media (max-width: 768px) { .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; } .data-table { font-size: 0.55rem; } }
    </style>
</head>
<body>
<div class="dashboard-container">
    <div class="header">
        <div class="logo"><h1><i class="fas fa-school"></i> EMIS Pro</h1><p>AEP | ASR | Conventional | GPA · Rank · Attendance · Dropout Management</p></div>
        <div class="stats-badge">
            <div class="badge"><div class="number" id="totalStudents">0</div><div class="label">Total Students</div></div>
            <div class="badge"><div class="number" id="totalAEP">0</div><div class="label">AEP</div></div>
            <div class="badge"><div class="number" id="totalASR">0</div><div class="label">ASR</div></div>
            <div class="badge"><div class="number" id="totalConventional">0</div><div class="label">Conventional</div></div>
            <div class="badge"><div class="number" id="totalCWD">0</div><div class="label">CWD</div></div>
            <div class="badge"><div class="number" id="avgGPA">0.00</div><div class="label">Avg GPA</div></div>
        </div>
    </div>

    <div class="nav-tabs">
        <button class="nav-btn active" data-section="register"><i class="fas fa-user-plus"></i> Student Registration</button>
        <button class="nav-btn" data-section="academic"><i class="fas fa-chart-line"></i> Academic & GPA</button>
        <button class="nav-btn" data-section="attendance"><i class="fas fa-calendar-check"></i> Attendance</button>
        <button class="nav-btn" data-section="dropout"><i class="fas fa-user-slash"></i> Dropout Management</button>
        <button class="nav-btn" data-section="roster"><i class="fas fa-table-list"></i> Subject Roster & Rank</button>
        <button class="nav-btn" data-section="reports"><i class="fas fa-file-alt"></i> Reports</button>
        <button class="nav-btn" data-section="settings"><i class="fas fa-cog"></i> Settings</button>
    </div>

    <!-- STUDENT REGISTRATION SECTION -->
    <div id="register" class="section active">
        <div class="grid-2">
            <div class="card">
                <div class="card-title"><i class="fas fa-user-graduate"></i> Student Registration Form</div>
                <div class="grid-2">
                    <div>
                        <div class="form-group"><label>Woreda Name (Aanaa)</label><input type="text" id="regWoreda" placeholder="e.g., Leka Dulecha, Gimbi, Kuyu"></div>
                        <div class="form-group"><label>School Name</label><input type="text" id="regSchool" placeholder="e.g., Ka'umsa Primary School"></div>
                        <div class="form-group"><label>Child Full Name</label><input type="text" id="regName" placeholder="Full Name"></div>
                        <div class="form-group"><label>Age</label><input type="number" id="regAge" placeholder="Age in years"></div>
                        <div class="form-group"><label>Sex</label><select id="regSex"><option value="M">Male (Dhiirra)</option><option value="F">Female (Dubartii)</option></select></div>
                        <div class="form-group"><label>Program Type</label><select id="regProgram"><option value="AEP">AEP (Accelerated Education Program)</option><option value="ASR">ASR (Accelerated School Readiness)</option><option value="Conventional">Conventional (Regular)</option></select></div>
                    </div>
                    <div>
                        <div class="form-group"><label>Beneficiary Type</label><select id="regBeneficiary"><option value="Host">Host Community</option><option value="IDP">IDP (Internally Displaced)</option><option value="Returnee">Returnee</option></select></div>
                        <div class="form-group"><label>Disability Status (CWD)</label><select id="regCWD"><option value="No">No</option><option value="Yes-Visual">Yes - Visual Impairment</option><option value="Yes-Hearing">Yes - Hearing Impairment</option><option value="Yes-Physical">Yes - Physical Disability</option><option value="Yes-Intellectual">Yes - Intellectual Disability</option></select></div>
                        <div class="form-group"><label>Vulnerability Category</label><select id="regVulnerability"><option value="None">None</option><option value="Orphan-Paternal">Orphan - Paternal (Father deceased)</option><option value="Orphan-Maternal">Orphan - Maternal (Mother deceased)</option><option value="Orphan-Both">Orphan - Both Parents</option><option value="Separated">Separated from Family</option><option value="UASC">UASC (Unaccompanied Child)</option></select></div>
                        <div class="form-group"><label>Grade/Level</label><select id="regGrade"><option value="KG1">KG1</option><option value="KG2">KG2</option><option value="KG3">KG3</option><option value="Grade1">Grade 1</option><option value="Grade2">Grade 2</option><option value="Grade3">Grade 3</option><option value="Grade4">Grade 4</option><option value="Grade5">Grade 5</option><option value="Grade6">Grade 6</option><option value="Grade7">Grade 7</option><option value="Grade8">Grade 8</option><option value="AEP-Level1">AEP Level 1</option><option value="AEP-Level2">AEP Level 2</option><option value="AEP-Level3">AEP Level 3</option></select></div>
                        <div class="form-group"><label>Admission Date</label><input type="date" id="regAdmissionDate"></div>
                    </div>
                </div>
                <button class="btn btn-primary" onclick="registerStudent()"><i class="fas fa-save"></i> Register Student</button>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-table"></i> Registered Students Summary</div>
                <div style="overflow-x: auto; max-height: 400px; overflow-y: auto;">
                    <table class="data-table" id="studentsTable">
                        <thead><tr><th>ID</th><th>Name</th><th>Woreda</th><th>School</th><th>Program</th><th>Beneficiary</th><th>CWD</th><th>Vulnerability</th><th>Actions</th></tr></thead>
                        <tbody id="studentsTableBody"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-chart-pie"></i> Registration Summary by Category</div>
            <div class="grid-3" id="summaryCards"></div>
        </div>
    </div>

    <!-- ACADEMIC & GPA SECTION -->
    <div id="academic" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-calculator"></i> Enter Academic Scores (Subjects)</div>
            <div class="form-group"><label>Select Student</label><select id="academicStudentSelect"></select></div>
            <div class="form-group"><label>Academic Term</label><select id="academicTerm"><option value="Term1">Term 1</option><option value="Term2">Term 2</option><option value="Final">Final Exam</option></select></div>
            <div class="form-group"><label>Academic Year</label><input type="text" id="academicYear" placeholder="e.g., 2024/25"></div>
            
            <div class="grid-3">
                <div class="form-group"><label>Afaan Oromoo</label><input type="number" id="scoreOromo" placeholder="0-100" step="0.01"></div>
                <div class="form-group"><label>English</label><input type="number" id="scoreEnglish" placeholder="0-100" step="0.01"></div>
                <div class="form-group"><label>Mathematics</label><input type="number" id="scoreMath" placeholder="0-100" step="0.01"></div>
                <div class="form-group"><label>Science</label><input type="number" id="scoreScience" placeholder="0-100" step="0.01"></div>
                <div class="form-group"><label>Social Studies</label><input type="number" id="scoreSocial" placeholder="0-100" step="0.01"></div>
                <div class="form-group"><label>Life Skills</label><input type="number" id="scoreLifeSkills" placeholder="0-100" step="0.01"></div>
            </div>
            <button class="btn btn-success" onclick="saveAcademicScores()"><i class="fas fa-save"></i> Save Academic Scores</button>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-chart-line"></i> GPA Analysis & Class Ranking</div>
            <div style="overflow-x: auto;">
                <table class="data-table" id="gpaTable">
                    <thead><tr><th>Rank</th><th>Student Name</th><th>Program</th><th>Grade</th><th>Average Score</th><th>GPA (4.0 Scale)</th><th>Status</th></tr></thead>
                    <tbody id="gpaTableBody"></tbody>
                </table>
            </div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-chart-simple"></i> GPA Distribution</div>
            <div class="chart-container" style="height: 250px;"><canvas id="gpaDistributionChart"></canvas></div>
        </div>
    </div>

    <!-- ATTENDANCE SECTION -->
    <div id="attendance" class="section">
        <div class="grid-2">
            <div class="card">
                <div class="card-title"><i class="fas fa-calendar-plus"></i> Mark Attendance</div>
                <div class="form-group"><label>Select Student</label><select id="attendanceStudentSelect"></select></div>
                <div class="form-group"><label>Date</label><input type="date" id="attendanceDate"></div>
                <div class="form-group"><label>Status</label><select id="attendanceStatus"><option value="Present">Present</option><option value="Absent">Absent</option><option value="Late">Late</option><option value="Excused">Excused</option></select></div>
                <button class="btn btn-primary" onclick="markAttendance()"><i class="fas fa-check-circle"></i> Mark Attendance</button>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-chart-line"></i> Attendance Summary</div>
                <div id="attendanceSummary"></div>
                <div class="chart-container" style="height: 200px;"><canvas id="attendanceChart"></canvas></div>
            </div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-history"></i> Attendance History</div>
            <div style="overflow-x: auto; max-height: 300px; overflow-y: auto;">
                <table class="data-table" id="attendanceHistoryTable"><thead><tr><th>Student</th><th>Date</th><th>Status</th><th>Actions</th></tr></thead><tbody id="attendanceHistoryBody"></tbody></table>
            </div>
        </div>
    </div>

    <!-- DROPOUT MANAGEMENT SECTION -->
    <div id="dropout" class="section">
        <div class="grid-2">
            <div class="card">
                <div class="card-title"><i class="fas fa-user-slash"></i> Register Dropout</div>
                <div class="form-group"><label>Select Student</label><select id="dropoutStudentSelect"></select></div>
                <div class="form-group"><label>Dropout Date</label><input type="date" id="dropoutDate"></div>
                <div class="form-group"><label>Dropout Reason</label><select id="dropoutReason"><option value="Financial">Financial/Economic</option><option value="Marriage">Early Marriage</option><option value="Displacement">Displacement/Conflict</option><option value="Disability">Lack of Disability Support</option><option value="Distance">Distance to School</option><option value="Work">Child Labor/Work</option><option value="Family Illness">Family Illness</option><option value="Lack of Interest">Lack of Interest</option><option value="Other">Other</option></select></div>
                <div class="form-group"><label>Notes</label><textarea id="dropoutNotes" rows="3" placeholder="Additional information..."></textarea></div>
                <button class="btn btn-warning" onclick="registerDropout()"><i class="fas fa-exclamation-triangle"></i> Register Dropout</button>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-chart-pie"></i> Dropout Analysis</div>
                <div id="dropoutStats"></div>
                <div class="chart-container" style="height: 200px;"><canvas id="dropoutChart"></canvas></div>
            </div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> Dropout Register</div>
            <div style="overflow-x: auto;"><table class="data-table" id="dropoutTable"><thead><tr><th>Student</th><th>School</th><th>Program</th><th>Dropout Date</th><th>Reason</th><th>Actions</th></tr></thead><tbody id="dropoutTableBody"></tbody></table></div>
        </div>
    </div>

    <!-- SUBJECT ROSTER & RANK SECTION -->
    <div id="roster" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-table-list"></i> Subject-Wise Roster & Rank Calculation</div>
            <div class="form-group"><label>Select Grade/Level</label><select id="rosterGradeSelect"></select></div>
            <div class="form-group"><label>Select Subject</label><select id="rosterSubjectSelect"><option value="Afaan Oromoo">Afaan Oromoo</option><option value="English">English</option><option value="Mathematics">Mathematics</option><option value="Science">Science</option><option value="Social Studies">Social Studies</option><option value="Life Skills">Life Skills</option></select></div>
            <button class="btn btn-primary" onclick="generateRoster()"><i class="fas fa-chart-line"></i> Generate Roster & Rank</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-trophy"></i> Subject Rank Table</div>
            <div style="overflow-x: auto;"><table class="data-table" id="rosterTable"><thead><tr><th>Rank</th><th>Student Name</th><th>Score</th><th>Grade</th><th>Program</th></tr></thead><tbody id="rosterTableBody"></tbody></table></div>
        </div>
    </div>

    <!-- REPORTS SECTION -->
    <div id="reports" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-file-pdf"></i> Generate Student Report Card</div>
            <div class="form-group"><label>Select Student</label><select id="reportStudentSelect"></select></div>
            <div class="form-group"><label>Format</label><select id="reportFormat"><option value="html">HTML Report</option><option value="pdf">PDF Report</option></select></div>
            <button class="btn btn-primary" onclick="generateReportCard()"><i class="fas fa-download"></i> Generate Report Card</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-chart-bar"></i> School Performance Dashboard</div>
            <div id="schoolDashboard"></div>
        </div>
    </div>

    <!-- SETTINGS SECTION -->
    <div id="settings" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-database"></i> Data Management</div>
            <button class="btn btn-secondary" onclick="exportAllData()"><i class="fas fa-download"></i> Export All Data (JSON)</button>
            <button class="btn btn-danger" onclick="clearAllData()"><i class="fas fa-trash"></i> Clear All Data</button>
            <button class="btn btn-info" onclick="loadSampleData()"><i class="fas fa-chalkboard"></i> Load Sample Data</button>
        </div>
    </div>
    <div class="footer"><p>EMIS Pro | Comprehensive Student Management | AEP · ASR · Conventional | GPA · Rank · Attendance · Dropout | Vulnerability Tracking (CWD, IDP, Orphan, UASC)</p></div>
</div>

<script>
    // Data Structures
    let students = [];
    let academicRecords = [];
    let attendanceRecords = [];
    let dropoutRecords = [];

    function saveData() {
        localStorage.setItem('emis_students', JSON.stringify(students));
        localStorage.setItem('emis_academic', JSON.stringify(academicRecords));
        localStorage.setItem('emis_attendance', JSON.stringify(attendanceRecords));
        localStorage.setItem('emis_dropout', JSON.stringify(dropoutRecords));
    }

    function loadData() {
        students = JSON.parse(localStorage.getItem('emis_students') || '[]');
        academicRecords = JSON.parse(localStorage.getItem('emis_academic') || '[]');
        attendanceRecords = JSON.parse(localStorage.getItem('emis_attendance') || '[]');
        dropoutRecords = JSON.parse(localStorage.getItem('emis_dropout') || '[]');
        updateAll();
    }

    function updateAll() {
        updateStats();
        updateStudentsTable();
        updateStudentSelects();
        updateGPATable();
        updateAttendanceHistory();
        updateAttendanceSummary();
        updateDropoutTable();
        updateDropoutStats();
        updateRosterGrades();
        updateSummaryCards();
        updateGPADistributionChart();
        updateAttendanceChart();
        updateDropoutChart();
        updateSchoolDashboard();
    }

    function updateStats() {
        document.getElementById('totalStudents').textContent = students.length;
        document.getElementById('totalAEP').textContent = students.filter(s => s.program === 'AEP').length;
        document.getElementById('totalASR').textContent = students.filter(s => s.program === 'ASR').length;
        document.getElementById('totalConventional').textContent = students.filter(s => s.program === 'Conventional').length;
        document.getElementById('totalCWD').textContent = students.filter(s => s.cwd !== 'No').length;
        let totalGPA = 0; let count = 0;
        students.forEach(s => { const record = academicRecords.find(a => a.studentId === s.id && a.term === 'Final'); if(record) { totalGPA += record.gpa; count++; } });
        document.getElementById('avgGPA').textContent = count > 0 ? (totalGPA / count).toFixed(2) : '0.00';
    }

    function registerStudent() {
        const student = {
            id: 'STU_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4).toUpperCase(),
            woreda: document.getElementById('regWoreda').value,
            school: document.getElementById('regSchool').value,
            name: document.getElementById('regName').value,
            age: parseInt(document.getElementById('regAge').value),
            sex: document.getElementById('regSex').value,
            program: document.getElementById('regProgram').value,
            beneficiary: document.getElementById('regBeneficiary').value,
            cwd: document.getElementById('regCWD').value,
            vulnerability: document.getElementById('regVulnerability').value,
            grade: document.getElementById('regGrade').value,
            admissionDate: document.getElementById('regAdmissionDate').value,
            status: 'Active',
            registrationDate: new Date().toISOString()
        };
        if(!student.woreda || !student.school || !student.name) { alert('Please fill Woreda, School, and Name'); return; }
        students.push(student);
        saveData();
        updateAll();
        clearRegistrationForm();
        alert('Student registered successfully! ID: ' + student.id);
    }

    function clearRegistrationForm() {
        document.getElementById('regWoreda').value = '';
        document.getElementById('regSchool').value = '';
        document.getElementById('regName').value = '';
        document.getElementById('regAge').value = '';
        document.getElementById('regAdmissionDate').value = '';
    }

    function updateStudentsTable() {
        const tbody = document.getElementById('studentsTableBody');
        if(students.length === 0) { tbody.innerHTML = '<tr><td colspan="9" style="text-align:center;">No students registered</td></tr>'; return; }
        tbody.innerHTML = '';
        students.forEach(s => {
            tbody.innerHTML += `<tr>
                <td>${s.id.substring(0,10)}</td><td>${escapeHtml(s.name)}</td><td>${escapeHtml(s.woreda)}</td><td>${escapeHtml(s.school)}</td>
                <td>${s.program}</td><td>${s.beneficiary}</td><td>${s.cwd !== 'No' ? '✓ CWD' : '-'}</td><td>${s.vulnerability !== 'None' ? s.vulnerability : '-'}</td>
                <td><button class="btn btn-danger btn-sm" onclick="deleteStudent('${s.id}')"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
    }

    function deleteStudent(id) { if(confirm('Delete student and all records?')) { students = students.filter(s => s.id !== id); academicRecords = academicRecords.filter(a => a.studentId !== id); attendanceRecords = attendanceRecords.filter(a => a.studentId !== id); dropoutRecords = dropoutRecords.filter(d => d.studentId !== id); saveData(); updateAll(); } }

    function updateStudentSelects() {
        const options = students.filter(s => s.status !== 'Dropped').map(s => `<option value="${s.id}">${s.name} - ${s.school} (${s.program})</option>`).join('');
        document.getElementById('academicStudentSelect').innerHTML = '<option value="">-- Select Student --</option>' + options;
        document.getElementById('attendanceStudentSelect').innerHTML = '<option value="">-- Select Student --</option>' + options;
        document.getElementById('dropoutStudentSelect').innerHTML = '<option value="">-- Select Student --</option>' + students.map(s => `<option value="${s.id}">${s.name} - ${s.school}</option>`).join('');
        document.getElementById('reportStudentSelect').innerHTML = '<option value="">-- Select Student --</option>' + options;
    }

    function saveAcademicScores() {
        const studentId = document.getElementById('academicStudentSelect').value;
        const term = document.getElementById('academicTerm').value;
        const year = document.getElementById('academicYear').value;
        if(!studentId) { alert('Select a student'); return; }
        const scores = {
            oromo: parseFloat(document.getElementById('scoreOromo').value) || 0,
            english: parseFloat(document.getElementById('scoreEnglish').value) || 0,
            math: parseFloat(document.getElementById('scoreMath').value) || 0,
            science: parseFloat(document.getElementById('scoreScience').value) || 0,
            social: parseFloat(document.getElementById('scoreSocial').value) || 0,
            lifeSkills: parseFloat(document.getElementById('scoreLifeSkills').value) || 0
        };
        const total = scores.oromo + scores.english + scores.math + scores.science + scores.social + scores.lifeSkills;
        const average = total / 6;
        const gpa = (average / 100) * 4;
        const existingIndex = academicRecords.findIndex(a => a.studentId === studentId && a.term === term);
        const record = { studentId, term, year, scores, total, average, gpa, date: new Date().toISOString() };
        if(existingIndex >= 0) academicRecords[existingIndex] = record;
        else academicRecords.push(record);
        saveData();
        updateAll();
        alert(`Academic scores saved! Average: ${average.toFixed(1)}%, GPA: ${gpa.toFixed(2)}`);
        clearAcademicForm();
    }

    function clearAcademicForm() {
        ['scoreOromo','scoreEnglish','scoreMath','scoreScience','scoreSocial','scoreLifeSkills'].forEach(id => document.getElementById(id).value = '');
        document.getElementById('academicYear').value = '';
    }

    function updateGPATable() {
        const tbody = document.getElementById('gpaTableBody');
        const studentGPAs = [];
        students.forEach(s => {
            const finalRecord = academicRecords.find(a => a.studentId === s.id && a.term === 'Final');
            if(finalRecord && s.status !== 'Dropped') studentGPAs.push({ student: s, average: finalRecord.average, gpa: finalRecord.gpa });
        });
        studentGPAs.sort((a,b) => b.gpa - a.gpa);
        tbody.innerHTML = '';
        studentGPAs.forEach((item, idx) => {
            const rankClass = idx === 0 ? 'rank-1' : (idx === 1 ? 'rank-2' : (idx === 2 ? 'rank-3' : ''));
            tbody.innerHTML += `<tr class="${rankClass}"><td>${idx + 1}</td><td>${escapeHtml(item.student.name)}</td><td>${item.student.program}</td><td>${item.student.grade}</td><td>${item.average.toFixed(1)}%</td><td>${item.gpa.toFixed(2)}</td><td>${item.gpa >= 3.5 ? '🌟 Excellent' : (item.gpa >= 2.5 ? '✅ Good' : (item.gpa >= 1.5 ? '⚠️ Satisfactory' : '🔴 Needs Improvement'))}</td></tr>`;
        });
        if(studentGPAs.length === 0) tbody.innerHTML = '<tr><td colspan="6" style="text-align:center;">No academic records. Add scores first.</td></tr>';
    }

    function markAttendance() {
        const studentId = document.getElementById('attendanceStudentSelect').value;
        const date = document.getElementById('attendanceDate').value;
        const status = document.getElementById('attendanceStatus').value;
        if(!studentId || !date) { alert('Select student and date'); return; }
        const existing = attendanceRecords.findIndex(a => a.studentId === studentId && a.date === date);
        const record = { studentId, date, status, timestamp: new Date().toISOString() };
        if(existing >= 0) attendanceRecords[existing] = record;
        else attendanceRecords.push(record);
        saveData();
        updateAll();
        alert(`Attendance marked as ${status} for ${date}`);
    }

    function updateAttendanceHistory() {
        const tbody = document.getElementById('attendanceHistoryBody');
        const recent = [...attendanceRecords].sort((a,b) => new Date(b.date) - new Date(a.date)).slice(0,50);
        tbody.innerHTML = '';
        recent.forEach(a => {
            const student = students.find(s => s.id === a.studentId);
            tbody.innerHTML += `<tr><td>${student ? escapeHtml(student.name) : 'N/A'}</td><td>${a.date}</td><td class="${a.status === 'Present' ? 'attendance-present' : 'attendance-absent'}">${a.status}</td><td><button class="btn btn-danger btn-sm" onclick="deleteAttendance('${a.studentId}','${a.date}')"><i class="fas fa-trash"></i></button></td></tr>`;
        });
    }

    function deleteAttendance(studentId, date) { attendanceRecords = attendanceRecords.filter(a => !(a.studentId === studentId && a.date === date)); saveData(); updateAll(); }

    function updateAttendanceSummary() {
        const studentId = document.getElementById('attendanceStudentSelect').value;
        if(!studentId) { document.getElementById('attendanceSummary').innerHTML = '<p>Select a student to view summary</p>'; return; }
        const studentRecords = attendanceRecords.filter(a => a.studentId === studentId);
        const total = studentRecords.length;
        const present = studentRecords.filter(a => a.status === 'Present').length;
        const absent = studentRecords.filter(a => a.status === 'Absent').length;
        const rate = total > 0 ? (present / total) * 100 : 0;
        document.getElementById('attendanceSummary').innerHTML = `<div class="alert alert-info"><strong>Attendance Summary</strong><br>Total Days: ${total}<br>Present: ${present}<br>Absent: ${absent}<br>Attendance Rate: ${rate.toFixed(1)}%<br>Status: ${rate >= 90 ? 'Excellent' : (rate >= 75 ? 'Good' : 'Needs Improvement')}</div>`;
    }

    function registerDropout() {
        const studentId = document.getElementById('dropoutStudentSelect').value;
        const date = document.getElementById('dropoutDate').value;
        const reason = document.getElementById('dropoutReason').value;
        const notes = document.getElementById('dropoutNotes').value;
        if(!studentId || !date) { alert('Select student and date'); return; }
        const student = students.find(s => s.id === studentId);
        if(student) student.status = 'Dropped';
        dropoutRecords.push({ id: 'DROP_' + Date.now(), studentId, studentName: student.name, school: student.school, program: student.program, dropoutDate: date, reason, notes });
        saveData();
        updateAll();
        alert(`Dropout registered for ${student.name}`);
        document.getElementById('dropoutDate').value = '';
        document.getElementById('dropoutNotes').value = '';
    }

    function updateDropoutTable() {
        const tbody = document.getElementById('dropoutTableBody');
        tbody.innerHTML = '';
        dropoutRecords.forEach(d => {
            tbody.innerHTML += `<tr><td>${escapeHtml(d.studentName)}</td><td>${escapeHtml(d.school)}</td><td>${d.program}</td><td>${d.dropoutDate}</td><td>${d.reason}</td><td><button class="btn btn-danger btn-sm" onclick="deleteDropout('${d.id}')"><i class="fas fa-trash"></i></button></td></tr>`;
        });
    }

    function deleteDropout(id) { dropoutRecords = dropoutRecords.filter(d => d.id !== id); saveData(); updateAll(); }

    function updateDropoutStats() {
        const byReason = {};
        dropoutRecords.forEach(d => { byReason[d.reason] = (byReason[d.reason] || 0) + 1; });
        document.getElementById('dropoutStats').innerHTML = `<div class="alert alert-warning"><strong>Dropout Statistics</strong><br>Total Dropouts: ${dropoutRecords.length}<br>By Program: AEP: ${dropoutRecords.filter(d=>d.program==='AEP').length}, ASR: ${dropoutRecords.filter(d=>d.program==='ASR').length}, Conventional: ${dropoutRecords.filter(d=>d.program==='Conventional').length}<br>Top Reason: ${Object.entries(byReason).sort((a,b)=>b[1]-a[1])[0]?.[0] || 'N/A'}</div>`;
    }

    function updateRosterGrades() { const select = document.getElementById('rosterGradeSelect'); select.innerHTML = '<option value="">-- Select Grade --</option>' + [...new Set(students.map(s => s.grade))].map(g => `<option value="${g}">${g}</option>`).join(''); }

    function generateRoster() {
        const grade = document.getElementById('rosterGradeSelect').value;
        const subject = document.getElementById('rosterSubjectSelect').value;
        if(!grade) { alert('Select grade'); return; }
        const gradeStudents = students.filter(s => s.grade === grade && s.status !== 'Dropped');
        const subjectMap = { 'Afaan Oromoo': 'oromo', 'English': 'english', 'Mathematics': 'math', 'Science': 'science', 'Social Studies': 'social', 'Life Skills': 'lifeSkills' };
        const scores = [];
        gradeStudents.forEach(s => {
            const finalRecord = academicRecords.find(a => a.studentId === s.id && a.term === 'Final');
            if(finalRecord) scores.push({ student: s, score: finalRecord.scores[subjectMap[subject]] || 0 });
        });
        scores.sort((a,b) => b.score - a.score);
        const tbody = document.getElementById('rosterTableBody');
        tbody.innerHTML = '';
        scores.forEach((item, idx) => { tbody.innerHTML += `<tr class="${idx === 0 ? 'rank-1' : ''}"><td>${idx + 1}</td><td>${escapeHtml(item.student.name)}</td><td>${item.score}</td><td>${item.student.grade}</td><td>${item.student.program}</td></tr>`; });
        if(scores.length === 0) tbody.innerHTML = '<tr><td colspan="5" style="text-align:center;">No academic records for this grade/subject</td></tr>';
    }

    function updateSummaryCards() {
        const container = document.getElementById('summaryCards');
        container.innerHTML = `
            <div class="alert alert-success"><strong>🏫 By Program</strong><br>AEP: ${students.filter(s=>s.program==='AEP').length}<br>ASR: ${students.filter(s=>s.program==='ASR').length}<br>Conventional: ${students.filter(s=>s.program==='Conventional').length}</div>
            <div class="alert alert-info"><strong>🫂 Beneficiary Type</strong><br>Host: ${students.filter(s=>s.beneficiary==='Host').length}<br>IDP: ${students.filter(s=>s.beneficiary==='IDP').length}<br>Returnee: ${students.filter(s=>s.beneficiary==='Returnee').length}</div>
            <div class="alert alert-warning"><strong>⚠️ Vulnerability</strong><br>Orphan: ${students.filter(s=>s.vulnerability.includes('Orphan')).length}<br>Separated/UASC: ${students.filter(s=>s.vulnerability==='Separated' || s.vulnerability==='UASC').length}<br>CWD: ${students.filter(s=>s.cwd!=='No').length}</div>`;
    }

    function updateGPADistributionChart() { const gpas = academicRecords.filter(a=>a.term==='Final').map(a=>a.gpa); if(window.gpaChart) window.gpaChart.destroy(); window.gpaChart = new Chart(document.getElementById('gpaDistributionChart'), { type: 'bar', data: { labels: ['0-1.0', '1.0-2.0', '2.0-3.0', '3.0-4.0'], datasets: [{ label: 'Number of Students', data: [gpas.filter(g=>g<1).length, gpas.filter(g=>g>=1 && g<2).length, gpas.filter(g=>g>=2 && g<3).length, gpas.filter(g=>g>=3).length], backgroundColor: '#3b82f6' }] } }); }
    function updateAttendanceChart() { const present = attendanceRecords.filter(a=>a.status==='Present').length; const absent = attendanceRecords.filter(a=>a.status==='Absent').length; if(window.attChart) window.attChart.destroy(); window.attChart = new Chart(document.getElementById('attendanceChart'), { type: 'doughnut', data: { labels: ['Present', 'Absent'], datasets: [{ data: [present, absent], backgroundColor: ['#10b981', '#ef4444'] }] } }); }
    function updateDropoutChart() { const reasons = {}; dropoutRecords.forEach(d=>{ reasons[d.reason]=(reasons[d.reason]||0)+1; }); if(window.dropChart) window.dropChart.destroy(); window.dropChart = new Chart(document.getElementById('dropoutChart'), { type: 'pie', data: { labels: Object.keys(reasons), datasets: [{ data: Object.values(reasons), backgroundColor: ['#ef4444','#f59e0b','#3b82f6','#10b981','#8b5cf6'] }] } }); }
    function updateSchoolDashboard() { const container = document.getElementById('schoolDashboard'); const schools = [...new Set(students.map(s=>s.school))]; let html = '<div class="grid-2">'; schools.forEach(school=>{ const schoolStudents = students.filter(st=>st.school===school); const avgGPA = academicRecords.filter(a=>schoolStudents.map(s=>s.id).includes(a.studentId) && a.term==='Final').reduce((s,a)=>s+a.gpa,0)/(schoolStudents.length||1); html += `<div class="alert alert-info"><strong>🏫 ${escapeHtml(school)}</strong><br>Students: ${schoolStudents.length}<br>Active: ${schoolStudents.filter(s=>s.status!=='Dropped').length}<br>Avg GPA: ${avgGPA.toFixed(2)}<br>Attendance Rate: ${(attendanceRecords.filter(a=>schoolStudents.map(s=>s.id).includes(a.studentId) && a.status==='Present').length / (attendanceRecords.filter(a=>schoolStudents.map(s=>s.id).includes(a.studentId)).length||1)*100).toFixed(1)}%</div>`; }); container.innerHTML = html + '</div>'; }
    function generateReportCard() {
        const studentId = document.getElementById('reportStudentSelect').value;
        const format = document.getElementById('reportFormat').value;
        const student = students.find(s => s.id === studentId);
        if(!student) { alert('Select a student'); return; }
        const finalRecord = academicRecords.find(a => a.studentId === studentId && a.term === 'Final');
        const attendance = attendanceRecords.filter(a => a.studentId === studentId);
        const present = attendance.filter(a => a.status === 'Present').length;
        const rate = attendance.length > 0 ? (present / attendance.length) * 100 : 0;
        const html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>Report Card - ${student.name}</title><style>body{font-family:Arial;margin:40px;}h1{color:#2563eb;}table{border-collapse:collapse;width:100%;}th,td{border:1px solid #ddd;padding:8px;}th{background:#f0f0f0;}</style></head><body><h1>📊 Student Report Card</h1><h2>${student.name}</h2><p><strong>Woreda:</strong> ${student.woreda} | <strong>School:</strong> ${student.school}<br><strong>Program:</strong> ${student.program} | <strong>Grade:</strong> ${student.grade}<br><strong>Beneficiary Type:</strong> ${student.beneficiary} | <strong>CWD Status:</strong> ${student.cwd}<br><strong>Vulnerability:</strong> ${student.vulnerability}</p><h3>Academic Performance (Final Term)</h3><table><tr><th>Subject</th><th>Score</th></tr>${finalRecord ? `<tr><td>Afaan Oromoo</td><td>${finalRecord.scores.oromo}</td></tr><tr><td>English</td><td>${finalRecord.scores.english}</td></tr><tr><td>Mathematics</td><td>${finalRecord.scores.math}</td></tr><tr><td>Science</td><td>${finalRecord.scores.science}</td></tr><tr><td>Social Studies</td><td>${finalRecord.scores.social}</td></tr><tr><td>Life Skills</td><td>${finalRecord.scores.lifeSkills}</td></tr><tr style="background:#f0f0f0;"><th>Average</th><th>${finalRecord.average.toFixed(1)}%</th></tr><tr style="background:#f0f0f0;"><th>GPA (4.0 Scale)</th><th>${finalRecord.gpa.toFixed(2)}</th></tr>` : '<tr><td colspan="2">No academic records found</td></tr>'}赶
            <h3>Attendance Summary</h3><p>Total Days: ${attendance.length}<br>Present: ${present}<br>Attendance Rate: ${rate.toFixed(1)}%</p>
            <h3>Grading Scale</h3><p>A (90-100%): Excellent | B (80-89%): Very Good | C (70-79%): Good | D (60-69%): Satisfactory | F (<60%): Needs Improvement</p>
            <p><small>Generated on ${new Date().toLocaleString()}</small></p></body></html>`;
        if(format === 'html') { const w=window.open(); w.document.write(html); w.document.close(); }
        else { const div=document.createElement('div'); div.innerHTML=html; document.body.appendChild(div); html2pdf().from(div).set({margin:1}).save(); setTimeout(()=>document.body.removeChild(div),1000); }
    }
    function exportAllData() { const a=document.createElement('a'); a.href=URL.createObjectURL(new Blob([JSON.stringify({students,academicRecords,attendanceRecords,dropoutRecords},null,2)],{type:'application/json'})); a.download=`emis_data.json`; a.click(); }
    function clearAllData() { if(confirm('Delete ALL data?')) { localStorage.clear(); students=[]; academicRecords=[]; attendanceRecords=[]; dropoutRecords=[]; updateAll(); alert('All data cleared.'); } }
    function loadSampleData() {
        students = [
            { id:'STU1', woreda:'Leka Dulecha', school:'Ka\'umsa Primary', name:'Almaz Tesfaye', age:12, sex:'F', program:'AEP', beneficiary:'IDP', cwd:'No', vulnerability:'Orphan-Paternal', grade:'Grade4', admissionDate:'2024-01-15', status:'Active' },
            { id:'STU2', woreda:'Gimbi', school:'Chuta Goch', name:'Biruk Abebe', age:11, sex:'M', program:'Conventional', beneficiary:'Host', cwd:'Yes-Visual', vulnerability:'None', grade:'Grade5', admissionDate:'2024-01-10', status:'Active' },
            { id:'STU3', woreda:'Kuyu', school:'Kesi Primary', name:'Chaltu Hussen', age:13, sex:'F', program:'ASR', beneficiary:'Returnee', cwd:'No', vulnerability:'UASC', grade:'Grade3', admissionDate:'2024-02-01', status:'Active' }
        ];
        academicRecords = [
            { studentId:'STU1', term:'Final', year:'2024/25', scores:{oromo:85,english:78,math:82,science:80,social:75,lifeSkills:88}, total:488, average:81.33, gpa:3.25 },
            { studentId:'STU2', term:'Final', year:'2024/25', scores:{oromo:92,english:88,math:90,science:85,social:82,lifeSkills:90}, total:527, average:87.83, gpa:3.51 }
        ];
        attendanceRecords = [
            { studentId:'STU1', date:'2025-03-10', status:'Present' }, { studentId:'STU1', date:'2025-03-11', status:'Present' }, { studentId:'STU2', date:'2025-03-10', status:'Present' }
        ];
        saveData(); updateAll(); alert('Sample data loaded!');
    }
    function escapeHtml(str){ if(!str) return ''; return str.replace(/[&<>]/g, m => m==='&'?'&amp;':m==='<'?'&lt;':'&gt;'); }
    document.querySelectorAll('.nav-btn').forEach(btn=>{ btn.addEventListener('click',()=>{ document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); document.querySelectorAll('.section').forEach(s=>s.classList.remove('active')); document.getElementById(btn.dataset.section).classList.add('active'); }); });
    loadData();
</script>
</body>
</html>
