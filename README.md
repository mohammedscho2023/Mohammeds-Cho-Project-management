<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaRL Department | English & Afaan Oromoo | Teaching at the Right Level</title>
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
        .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 15px; }
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
        .formula-box { background: #fef3c7; padding: 8px; border-radius: 8px; font-family: monospace; font-size: 0.7rem; margin-top: 8px; }
        .footer { text-align: center; padding: 20px; color: rgba(255,255,255,0.5); font-size: 0.7rem; }
        .literacy-level { display: inline-block; padding: 2px 8px; border-radius: 20px; font-size: 0.65rem; font-weight: 600; }
        .level-1 { background: #fee2e2; color: #991b1b; }
        .level-2 { background: #fed7aa; color: #9a3412; }
        .level-3 { background: #fef3c7; color: #92400e; }
        .level-4 { background: #d1fae5; color: #065f46; }
        .level-5 { background: #dbeafe; color: #1e40af; }
        @media (max-width: 768px) { .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; } .data-table { font-size: 0.55rem; } }
    </style>
</head>
<body>
<div class="dashboard-container">
    <div class="header">
        <div class="logo"><h1><i class="fas fa-language"></i> TaRL Department</h1><p>Teaching at the Right Level | English & Afaan Oromoo | 5 Components Each</p></div>
        <div class="stats-badge">
            <div class="badge"><div class="number" id="totalStudents">0</div><div class="label">Total Students</div></div>
            <div class="badge"><div class="number" id="totalSchools">0</div><div class="label">Schools</div></div>
            <div class="badge"><div class="number" id="avgEnglish">0%</div><div class="label">Avg English</div></div>
            <div class="badge"><div class="number" id="avgOromo">0%</div><div class="label">Avg Afaan Oromoo</div></div>
        </div>
    </div>

    <div class="nav-tabs">
        <button class="nav-btn active" data-section="dashboard"><i class="fas fa-chart-pie"></i> Dashboard</button>
        <button class="nav-btn" data-section="woreda"><i class="fas fa-map-marker-alt"></i> Woreda Level</button>
        <button class="nav-btn" data-section="school"><i class="fas fa-school"></i> School Level</button>
        <button class="nav-btn" data-section="student"><i class="fas fa-user-graduate"></i> Student Registration</button>
        <button class="nav-btn" data-section="assessment"><i class="fas fa-clipboard-list"></i> Assessment Tools</button>
        <button class="nav-btn" data-section="reports"><i class="fas fa-file-alt"></i> Reports</button>
        <button class="nav-btn" data-section="import"><i class="fas fa-upload"></i> Import Data</button>
        <button class="nav-btn" data-section="settings"><i class="fas fa-cog"></i> Settings</button>
    </div>

    <!-- DASHBOARD SECTION -->
    <div id="dashboard" class="section active">
        <div class="grid-3">
            <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> English Progress</div><div class="chart-container" style="height:250px;"><canvas id="englishChart"></canvas></div></div>
            <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> Afaan Oromoo Progress</div><div class="chart-container" style="height:250px;"><canvas id="oromoChart"></canvas></div></div>
            <div class="card"><div class="card-title"><i class="fas fa-chart-pie"></i> English Level Distribution</div><div class="chart-container" style="height:250px;"><canvas id="englishLevelChart"></canvas></div></div>
        </div>
        <div class="grid-2">
            <div class="card"><div class="card-title"><i class="fas fa-chart-bar"></i> Performance by Grade</div><div class="chart-container" style="height:250px;"><canvas id="gradePerformanceChart"></canvas></div></div>
            <div class="card"><div class="card-title"><i class="fas fa-chart-simple"></i> Baseline vs Current Progress</div><div class="chart-container" style="height:250px;"><canvas id="baselineProgressChart"></canvas></div></div>
        </div>
        <div class="card"><div class="card-title"><i class="fas fa-tachometer-alt"></i> TaRL Program Summary</div><div id="programSummary"></div></div>
    </div>

    <!-- WOREDA LEVEL DATA SECTION -->
    <div id="woreda" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> Add Woreda Level Data</div>
            <div class="grid-3">
                <div class="form-group"><label>Zone (Godina)</label><input type="text" id="woredaZone" placeholder="e.g., East Wolega"></div>
                <div class="form-group"><label>Woreda (Aanaa)</label><input type="text" id="woredaName" placeholder="e.g., Leka Dulecha"></div>
                <div class="form-group"><label>Number of Schools</label><input type="number" id="woredaSchools" placeholder="0"></div>
                <div class="form-group"><label>Assessment Phase</label><select id="woredaPhase"><option value="baseline">Baseline (Ka'umsa)</option><option value="midline">Midline (Jidduu)</option><option value="endline">Endline (Xumura)</option></select></div>
                <div class="form-group"><label>Grade</label><select id="woredaGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                <div class="form-group"><label>Total Students</label><input type="number" id="woredaTotalStudents" placeholder="0"></div>
            </div>
            <div class="grid-2">
                <!-- ENGLISH SECTION (replaces Maths) - 5 components -->
                <div><h4><i class="fas fa-language"></i> English (5 Components)</h4>
                    <div class="form-group"><label>Gulantaa Jalqabaa (Initial Grade / Baseline)</label><input type="number" id="woredaEngInitial" placeholder="0"></div>
                    <div class="form-group"><label>Arfii (Alphabet / Letter Recognition)</label><input type="number" id="woredaEngAlphabet" placeholder="0"></div>
                    <div class="form-group"><label>Jechoota (Word Recognition / Vocabulary)</label><input type="number" id="woredaEngWords" placeholder="0"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Simple Sentence Reading)</label><input type="number" id="woredaEngSentence" placeholder="0"></div>
                    <div class="form-group"><label>Seenessaa (Story / Reading Comprehension)</label><input type="number" id="woredaEngStory" placeholder="0"></div>
                </div>
                <!-- AFAAN OROMOO SECTION - 5 components -->
                <div><h4><i class="fas fa-book"></i> Afaan Oromoo (5 Components)</h4>
                    <div class="form-group"><label>Gulantaa Jalqabaa (Initial Grade)</label><input type="number" id="woredaLitInitial" placeholder="0"></div>
                    <div class="form-group"><label>Qubee (Alphabet Recognition)</label><input type="number" id="woredaLitQubee" placeholder="0"></div>
                    <div class="form-group"><label>Jecha (Word Recognition)</label><input type="number" id="woredaLitWord" placeholder="0"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Simple Sentence)</label><input type="number" id="woredaLitSentence" placeholder="0"></div>
                    <div class="form-group"><label>Seenessaa (Story / Comprehension)</label><input type="number" id="woredaLitStory" placeholder="0"></div>
                </div>
            </div>
            <button class="btn btn-primary" onclick="addWoredaData()"><i class="fas fa-save"></i> Save Woreda Data</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-table"></i> Woreda Level Summary</div>
            <div style="overflow-x: auto;"><table class="data-table" id="woredaTable"><thead><tr><th>Zone</th><th>Woreda</th><th>Schools</th><th>Phase</th><th>Grade</th><th>Total Students</th><th>English Score</th><th>Afaan Oromoo Score</th><th>Actions</th></tr></thead><tbody id="woredaTableBody"></tbody></table></div>
        </div>
    </div>

    <!-- SCHOOL LEVEL DATA SECTION -->
    <div id="school" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> Add School Level Data</div>
            <div class="grid-3">
                <div class="form-group"><label>Zone (Godina)</label><input type="text" id="schoolZone" placeholder="e.g., East Wolega"></div>
                <div class="form-group"><label>Woreda (Aanaa)</label><input type="text" id="schoolWoreda" placeholder="e.g., Leka Dulecha"></div>
                <div class="form-group"><label>School ID</label><input type="text" id="schoolId" placeholder="School ID"></div>
                <div class="form-group"><label>School Name</label><input type="text" id="schoolName" placeholder="School Name"></div>
                <div class="form-group"><label>Grade (Kutaa)</label><select id="schoolGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                <div class="form-group"><label>Assessment Phase</label><select id="schoolPhase"><option value="baseline">Baseline</option><option value="midline">Midline</option><option value="endline">Endline</option></select></div>
            </div>
            <div class="grid-2">
                <div><h4><i class="fas fa-language"></i> English Scores (5 Components)</h4>
                    <div class="form-group"><label>Gulantaa Jalqabaa (Initial)</label><input type="number" id="schoolEngInitial" placeholder="0"></div>
                    <div class="form-group"><label>Arfii (Alphabet)</label><input type="number" id="schoolEngAlphabet" placeholder="0"></div>
                    <div class="form-group"><label>Jechoota (Words)</label><input type="number" id="schoolEngWords" placeholder="0"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Sentence)</label><input type="number" id="schoolEngSentence" placeholder="0"></div>
                    <div class="form-group"><label>Seenessaa (Story)</label><input type="number" id="schoolEngStory" placeholder="0"></div>
                </div>
                <div><h4><i class="fas fa-book"></i> Afaan Oromoo Scores (5 Components)</h4>
                    <div class="form-group"><label>Gulantaa Jalqabaa (Initial)</label><input type="number" id="schoolLitInitial" placeholder="0"></div>
                    <div class="form-group"><label>Qubee</label><input type="number" id="schoolLitQubee" placeholder="0"></div>
                    <div class="form-group"><label>Jecha</label><input type="number" id="schoolLitWord" placeholder="0"></div>
                    <div class="form-group"><label>Keeyyata Salphaa</label><input type="number" id="schoolLitSentence" placeholder="0"></div>
                    <div class="form-group"><label>Seenessaa</label><input type="number" id="schoolLitStory" placeholder="0"></div>
                </div>
            </div>
            <button class="btn btn-primary" onclick="addSchoolData()"><i class="fas fa-save"></i> Save School Data</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-table"></i> School Level Data</div>
            <div style="overflow-x: auto;"><table class="data-table" id="schoolTable"><thead><tr><th>Zone</th><th>Woreda</th><th>School Name</th><th>Grade</th><th>Phase</th><th>English Total</th><th>Afaan Oromoo Total</th><th>Actions</th></tr></thead><tbody id="schoolTableBody"></tbody></table></div>
        </div>
    </div>

    <!-- STUDENT REGISTRATION SECTION -->
    <div id="student" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-user-plus"></i> Register Student (Barataa)</div>
            <div class="grid-3">
                <div class="form-group"><label>Zone (Godina)</label><input type="text" id="studentZone" placeholder="e.g., East Wolega"></div>
                <div class="form-group"><label>Woreda (Aanaa)</label><input type="text" id="studentWoreda" placeholder="e.g., Leka Dulecha"></div>
                <div class="form-group"><label>School ID</label><input type="text" id="studentSchoolId" placeholder="School ID"></div>
                <div class="form-group"><label>School Name</label><input type="text" id="studentSchoolName" placeholder="School Name"></div>
                <div class="form-group"><label>Learner ID</label><input type="text" id="learnerId" placeholder="Auto-generated" readonly></div>
                <div class="form-group"><label>Student Name</label><input type="text" id="studentName" placeholder="Full Name"></div>
                <div class="form-group"><label>Sex</label><select id="studentSex"><option value="M">Dhiirra (Male)</option><option value="F">Dubartii (Female)</option></select></div>
                <div class="form-group"><label>Grade (Kutaa)</label><select id="studentGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                <div class="form-group"><label>Special Needs (Fedhii Addaa)</label><select id="specialNeeds"><option value="None">None</option><option value="Vision">Rakkoo Arguu (Vision)</option><option value="Hearing">Rakkoo Dhaga'uu (Hearing)</option><option value="Physical">Hirdhiina Qaamaa (Physical)</option><option value="Communication">Rakkoo Waliifgaluu (Communication)</option><option value="Cognitive">Hanqiina Guddina Sammu (Cognitive)</option><option value="Behavioral">Rakkoo Amalaa (Behavioral)</option></select></div>
            </div>
            <div class="grid-2">
                <!-- ENGLISH ASSESSMENT - 5 components -->
                <div><h4><i class="fas fa-language"></i> English Assessment (5 Components)</h4>
                    <div class="form-group"><label>Gulantaa Jalqabaa (Initial Grade)</label><input type="number" id="studentEngInitial" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Arfii (Alphabet Recognition)</label><input type="number" id="studentEngAlphabet" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Jechoota (Word Recognition)</label><input type="number" id="studentEngWords" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Simple Sentence)</label><input type="number" id="studentEngSentence" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Seenessaa (Story Comprehension)</label><input type="number" id="studentEngStory" placeholder="0/1" step="1" min="0" max="1"></div>
                </div>
                <!-- AFAAN OROMOO ASSESSMENT - 5 components -->
                <div><h4><i class="fas fa-book"></i> Afaan Oromoo Assessment (5 Components)</h4>
                    <div class="form-group"><label>Gulantaa Jalqabaa (Initial Grade)</label><input type="number" id="studentLitInitial" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Qubee (Alphabet Recognition)</label><input type="number" id="studentLitQubee" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Jecha (Word Recognition)</label><input type="number" id="studentLitWord" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Simple Sentence)</label><input type="number" id="studentLitSentence" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Seenessaa (Story Comprehension)</label><input type="number" id="studentLitStory" placeholder="0/1" step="1" min="0" max="1"></div>
                </div>
            </div>
            <button class="btn btn-primary" onclick="registerStudent()"><i class="fas fa-save"></i> Register Student</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-table-list"></i> Student Roster (Kutaa 3-5)</div>
            <div style="overflow-x: auto; max-height: 400px; overflow-y: auto;"><table class="data-table" id="studentTable"><thead><tr><th>ID</th><th>Name</th><th>Zone</th><th>Woreda</th><th>School</th><th>Grade</th><th>Sex</th><th>English Score</th><th>Afaan Oromoo Score</th><th>English Level</th><th>Actions</th></tr></thead><tbody id="studentTableBody"></tbody></table></div>
        </div>
    </div>

    <!-- ASSESSMENT TOOLS SECTION -->
    <div id="assessment" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-toolbox"></i> TaRL Assessment Tools Reference</div>
            <div class="grid-3">
                <div class="alert alert-info"><strong>📖 English Levels (6 Levels)</strong><br>1_Beginner → 2_Alphabet → 3_Words → 4_Sentence → 5_Story → 6_Computed</div>
                <div class="alert alert-info"><strong>📖 Afaan Oromoo Levels (6 Levels)</strong><br>1_Beginner → 2_Qubee → 3_Jecha → 4_Keeyyata → 5_Seenessaa → 6_Computed</div>
                <div class="alert alert-info"><strong>📋 Formulas</strong><br>English Total = Gulantaa Jalqabaa + Arfii + Jechoota + Keeyyata Salphaa + Seenessaa<br>Afaan Oromoo Total = Gulantaa Jalqabaa + Qubee + Jecha + Keeyyata Salphaa + Seenessaa</div>
            </div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-chart-line"></i> Assessment Progress Tracking</div>
            <div class="grid-2">
                <div class="chart-container" style="height:250px;"><canvas id="assessmentProgressChart"></canvas></div>
                <div id="assessmentStats"></div>
            </div>
        </div>
    </div>

    <!-- REPORTS SECTION -->
    <div id="reports" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-file-pdf"></i> Generate TaRL Report</div>
            <div class="grid-2">
                <div><div class="form-group"><label>Report Type</label><select id="reportType"><option value="woreda">Woreda Level Summary</option><option value="school">School Level Summary</option><option value="student">Student Roster</option></select></div></div>
                <div><div class="form-group"><label>Format</label><select id="reportFormat"><option value="html">HTML Report</option><option value="pdf">PDF Report</option></select></div></div>
            </div>
            <button class="btn btn-primary" onclick="generateTaRLReport()"><i class="fas fa-download"></i> Generate Report</button>
        </div>
    </div>

    <!-- IMPORT SECTION -->
    <div id="import" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-file-excel"></i> Import Excel Data</div>
            <div class="upload-area" onclick="document.getElementById('excelFile').click()" style="border:2px dashed #ccc; border-radius:20px; padding:40px; text-align:center; cursor:pointer;"><i class="fas fa-cloud-upload-alt" style="font-size:3rem; color:#2563eb;"></i><p>Click to upload Excel file (.xlsx, .xls, .csv)</p><input type="file" id="excelFile" accept=".xlsx,.xls,.csv" style="display:none;" onchange="importExcelData(this)"></div>
            <div id="importPreview"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-database"></i> Load Sample TaRL Data</div>
            <button class="btn btn-success" onclick="loadSampleData()"><i class="fas fa-chalkboard"></i> Load Sample Data (English & Afaan Oromoo)</button>
        </div>
    </div>

    <!-- SETTINGS SECTION -->
    <div id="settings" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-database"></i> Data Management</div>
            <button class="btn btn-secondary" onclick="exportAllData()"><i class="fas fa-download"></i> Export All Data (JSON)</button>
            <button class="btn btn-danger" onclick="clearAllData()"><i class="fas fa-trash"></i> Clear All Data</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-info-circle"></i> About TaRL Department</div>
            <p><strong>Teaching at the Right Level (TaRL) - English & Afaan Oromoo</strong><br>
            Sadarkaa Aanaalee fi Manneen Barnootaa itti Sassaabamu<br>
            Kutaa 3ffaa, 4ffaa fi 5ffaa<br><br>
            <strong>English Components (5 parts):</strong><br>
            1. Gulantaa Jalqabaa (Initial Grade / Baseline)<br>
            2. Arfii (Alphabet / Letter Recognition)<br>
            3. Jechoota (Word Recognition / Vocabulary)<br>
            4. Keeyyata Salphaa (Simple Sentence Reading)<br>
            5. Seenessaa (Story / Reading Comprehension)<br><br>
            <strong>Afaan Oromoo Components (5 parts):</strong><br>
            1. Gulantaa Jalqabaa (Initial Grade)<br>
            2. Qubee (Alphabet Recognition)<br>
            3. Jecha (Word Recognition)<br>
            4. Keeyyata Salphaa (Simple Sentence)<br>
            5. Seenessaa (Story / Comprehension)</p>
        </div>
    </div>
    <div class="footer"><p>TaRL Department | English & Afaan Oromoo | 5 Components Each | Baseline · Midline · Endline</p></div>
</div>

<script>
    // Data Structures
    let woredaData = [];
    let schoolData = [];
    let students = [];

    function saveData() {
        localStorage.setItem('tarl_woreda', JSON.stringify(woredaData));
        localStorage.setItem('tarl_school', JSON.stringify(schoolData));
        localStorage.setItem('tarl_students', JSON.stringify(students));
    }

    function loadData() {
        woredaData = JSON.parse(localStorage.getItem('tarl_woreda') || '[]');
        schoolData = JSON.parse(localStorage.getItem('tarl_school') || '[]');
        students = JSON.parse(localStorage.getItem('tarl_students') || '[]');
        updateAll();
    }

    function updateAll() {
        updateStats();
        updateWoredaTable();
        updateSchoolTable();
        updateStudentTable();
        updateCharts();
        updateProgramSummary();
    }

    function updateStats() {
        document.getElementById('totalStudents').textContent = students.length;
        const uniqueSchools = [...new Set(schoolData.map(s => s.schoolName))];
        document.getElementById('totalSchools').textContent = uniqueSchools.length;
        let engTotal = 0, oromoTotal = 0;
        students.forEach(s => { engTotal += s.englishScore || 0; oromoTotal += s.oromoScore || 0; });
        document.getElementById('avgEnglish').textContent = students.length ? ((engTotal / (students.length * 5)) * 100).toFixed(0) : '0';
        document.getElementById('avgOromo').textContent = students.length ? ((oromoTotal / (students.length * 5)) * 100).toFixed(0) : '0';
    }

    function addWoredaData() {
        const engTotal = (parseInt(document.getElementById('woredaEngInitial').value)||0) + (parseInt(document.getElementById('woredaEngAlphabet').value)||0) + 
                         (parseInt(document.getElementById('woredaEngWords').value)||0) + (parseInt(document.getElementById('woredaEngSentence').value)||0) + 
                         (parseInt(document.getElementById('woredaEngStory').value)||0);
        const litTotal = (parseInt(document.getElementById('woredaLitInitial').value)||0) + (parseInt(document.getElementById('woredaLitQubee').value)||0) + 
                         (parseInt(document.getElementById('woredaLitWord').value)||0) + (parseInt(document.getElementById('woredaLitSentence').value)||0) + 
                         (parseInt(document.getElementById('woredaLitStory').value)||0);
        const data = {
            id: 'WRD_' + Date.now(),
            zone: document.getElementById('woredaZone').value,
            woreda: document.getElementById('woredaName').value,
            schools: parseInt(document.getElementById('woredaSchools').value) || 0,
            phase: document.getElementById('woredaPhase').value,
            grade: document.getElementById('woredaGrade').value,
            totalStudents: parseInt(document.getElementById('woredaTotalStudents').value) || 0,
            english: { initial: parseInt(document.getElementById('woredaEngInitial').value)||0, alphabet: parseInt(document.getElementById('woredaEngAlphabet').value)||0, words: parseInt(document.getElementById('woredaEngWords').value)||0, sentence: parseInt(document.getElementById('woredaEngSentence').value)||0, story: parseInt(document.getElementById('woredaEngStory').value)||0, total: engTotal },
            oromo: { initial: parseInt(document.getElementById('woredaLitInitial').value)||0, qubee: parseInt(document.getElementById('woredaLitQubee').value)||0, word: parseInt(document.getElementById('woredaLitWord').value)||0, sentence: parseInt(document.getElementById('woredaLitSentence').value)||0, story: parseInt(document.getElementById('woredaLitStory').value)||0, total: litTotal }
        };
        woredaData.push(data);
        saveData(); updateAll(); alert('Woreda data saved!');
    }

    function addSchoolData() {
        const engTotal = (parseInt(document.getElementById('schoolEngInitial').value)||0) + (parseInt(document.getElementById('schoolEngAlphabet').value)||0) + 
                         (parseInt(document.getElementById('schoolEngWords').value)||0) + (parseInt(document.getElementById('schoolEngSentence').value)||0) + 
                         (parseInt(document.getElementById('schoolEngStory').value)||0);
        const litTotal = (parseInt(document.getElementById('schoolLitInitial').value)||0) + (parseInt(document.getElementById('schoolLitQubee').value)||0) + 
                         (parseInt(document.getElementById('schoolLitWord').value)||0) + (parseInt(document.getElementById('schoolLitSentence').value)||0) + 
                         (parseInt(document.getElementById('schoolLitStory').value)||0);
        const data = {
            id: 'SCH_' + Date.now(),
            zone: document.getElementById('schoolZone').value,
            woreda: document.getElementById('schoolWoreda').value,
            schoolId: document.getElementById('schoolId').value,
            schoolName: document.getElementById('schoolName').value,
            grade: document.getElementById('schoolGrade').value,
            phase: document.getElementById('schoolPhase').value,
            english: { initial: parseInt(document.getElementById('schoolEngInitial').value)||0, alphabet: parseInt(document.getElementById('schoolEngAlphabet').value)||0, words: parseInt(document.getElementById('schoolEngWords').value)||0, sentence: parseInt(document.getElementById('schoolEngSentence').value)||0, story: parseInt(document.getElementById('schoolEngStory').value)||0, total: engTotal },
            oromo: { initial: parseInt(document.getElementById('schoolLitInitial').value)||0, qubee: parseInt(document.getElementById('schoolLitQubee').value)||0, word: parseInt(document.getElementById('schoolLitWord').value)||0, sentence: parseInt(document.getElementById('schoolLitSentence').value)||0, story: parseInt(document.getElementById('schoolLitStory').value)||0, total: litTotal }
        };
        schoolData.push(data);
        saveData(); updateAll(); alert('School data saved!');
    }

    function registerStudent() {
        const learnerId = 'LRN_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4).toUpperCase();
        document.getElementById('learnerId').value = learnerId;
        const englishScore = (parseInt(document.getElementById('studentEngInitial').value)||0) + (parseInt(document.getElementById('studentEngAlphabet').value)||0) + 
                             (parseInt(document.getElementById('studentEngWords').value)||0) + (parseInt(document.getElementById('studentEngSentence').value)||0) + 
                             (parseInt(document.getElementById('studentEngStory').value)||0);
        const oromoScore = (parseInt(document.getElementById('studentLitInitial').value)||0) + (parseInt(document.getElementById('studentLitQubee').value)||0) + 
                           (parseInt(document.getElementById('studentLitWord').value)||0) + (parseInt(document.getElementById('studentLitSentence').value)||0) + 
                           (parseInt(document.getElementById('studentLitStory').value)||0);
        const englishLevel = getEnglishLevel(englishScore);
        const oromoLevel = getOromoLevel(oromoScore);
        
        students.push({
            id: learnerId,
            zone: document.getElementById('studentZone').value,
            woreda: document.getElementById('studentWoreda').value,
            schoolId: document.getElementById('studentSchoolId').value,
            schoolName: document.getElementById('studentSchoolName').value,
            name: document.getElementById('studentName').value,
            sex: document.getElementById('studentSex').value,
            grade: document.getElementById('studentGrade').value,
            specialNeeds: document.getElementById('specialNeeds').value,
            englishScore: englishScore,
            oromoScore: oromoScore,
            englishLevel: englishLevel,
            oromoLevel: oromoLevel
        });
        saveData(); updateAll(); alert(`Student registered! ID: ${learnerId}`);
    }

    function getEnglishLevel(score) {
        if(score === 0) return { level: 'Beginner', class: 'level-1', text: '1_Beginner' };
        if(score === 1) return { level: 'Alphabet', class: 'level-2', text: '2_Alphabet' };
        if(score === 2) return { level: 'Words', class: 'level-3', text: '3_Words' };
        if(score === 3) return { level: 'Sentence', class: 'level-4', text: '4_Sentence' };
        if(score === 4) return { level: 'Story', class: 'level-5', text: '5_Story' };
        return { level: 'Computed', class: 'level-1', text: '6_Computed' };
    }

    function getOromoLevel(score) {
        if(score === 0) return { level: 'Beginner', class: 'level-1', text: '1_Beginner' };
        if(score === 1) return { level: 'Qubee', class: 'level-2', text: '2_Qubee' };
        if(score === 2) return { level: 'Jecha', class: 'level-3', text: '3_Jecha' };
        if(score === 3) return { level: 'Keeyyata', class: 'level-4', text: '4_Keeyyata' };
        if(score === 4) return { level: 'Seenessaa', class: 'level-5', text: '5_Seenessaa' };
        return { level: 'Computed', class: 'level-1', text: '6_Computed' };
    }

    function updateWoredaTable() {
        const tbody = document.getElementById('woredaTableBody');
        if(woredaData.length === 0) { tbody.innerHTML = '<tr><td colspan="9" style="text-align:center;">No woreda data. Add above.</td></tr>'; return; }
        tbody.innerHTML = '';
        woredaData.forEach(w => {
            tbody.innerHTML += `<tr>
                <td>${escapeHtml(w.zone)}</td>
                <td>${escapeHtml(w.woreda)}</td>
                <td>${w.schools}</td>
                <td>${w.phase}</td>
                <td>Kutaa ${w.grade}</td>
                <td>${w.totalStudents}</td>
                <td>${w.english.total} / 5 (${((w.english.total/5)*100).toFixed(0)}%)</td>
                <td>${w.oromo.total} / 5 (${((w.oromo.total/5)*100).toFixed(0)}%)</td>
                <td><button class="btn btn-danger btn-sm" onclick="deleteWoreda('${w.id}')"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
    }

    function updateSchoolTable() {
        const tbody = document.getElementById('schoolTableBody');
        if(schoolData.length === 0) { tbody.innerHTML = '<tr><td colspan="8" style="text-align:center;">No school data. Add above.</td></tr>'; return; }
        tbody.innerHTML = '';
        schoolData.forEach(s => {
            tbody.innerHTML += `<tr>
                <td>${escapeHtml(s.zone)}</td>
                <td>${escapeHtml(s.woreda)}</td>
                <td>${escapeHtml(s.schoolName)}</td>
                <td>Kutaa ${s.grade}</td>
                <td>${s.phase}</td>
                <td>${s.english.total} / 5 (${((s.english.total/5)*100).toFixed(0)}%)</td>
                <td>${s.oromo.total} / 5 (${((s.oromo.total/5)*100).toFixed(0)}%)</td>
                <td><button class="btn btn-danger btn-sm" onclick="deleteSchool('${s.id}')"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
    }

    function updateStudentTable() {
        const tbody = document.getElementById('studentTableBody');
        if(students.length === 0) { tbody.innerHTML = '<tr><td colspan="11" style="text-align:center;">No students registered. Add above.</td></tr>'; return; }
        tbody.innerHTML = '';
        students.forEach(s => {
            tbody.innerHTML += `<tr>
                <td>${s.id.substring(0,12)}</td>
                <td>${escapeHtml(s.name)}</td>
                <td>${escapeHtml(s.zone)}</td>
                <td>${escapeHtml(s.woreda)}</td>
                <td>${escapeHtml(s.schoolName)}</td>
                <td>Kutaa ${s.grade}</td>
                <td>${s.sex === 'M' ? 'Dhiirra' : 'Dubartii'}</td>
                <td>${s.englishScore}/5 (${((s.englishScore/5)*100).toFixed(0)}%)</td>
                <td>${s.oromoScore}/5 (${((s.oromoScore/5)*100).toFixed(0)}%)</td>
                <td><span class="literacy-level ${s.englishLevel.class}">${s.englishLevel.text}</span></td>
                <td><button class="btn btn-danger btn-sm" onclick="deleteStudent('${s.id}')"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
    }

    function updateCharts() {
        const phases = ['baseline', 'midline', 'endline'];
        const engData = phases.map(p => schoolData.filter(s => s.phase === p).reduce((sum, s) => sum + s.english.total, 0) / (schoolData.filter(s => s.phase === p).length || 1) * 20);
        const oromoData = phases.map(p => schoolData.filter(s => s.phase === p).reduce((sum, s) => sum + s.oromo.total, 0) / (schoolData.filter(s => s.phase === p).length || 1) * 20);
        if(window.engChart) window.engChart.destroy();
        window.engChart = new Chart(document.getElementById('englishChart'), { type: 'line', data: { labels: ['Baseline', 'Midline', 'Endline'], datasets: [{ label: 'English (%)', data: engData, borderColor: '#3b82f6', fill: true }] } });
        if(window.oromoChart) window.oromoChart.destroy();
        window.oromoChart = new Chart(document.getElementById('oromoChart'), { type: 'line', data: { labels: ['Baseline', 'Midline', 'Endline'], datasets: [{ label: 'Afaan Oromoo (%)', data: oromoData, borderColor: '#10b981', fill: true }] } });
        
        const grade3 = students.filter(s => s.grade === '3').length;
        const grade4 = students.filter(s => s.grade === '4').length;
        const grade5 = students.filter(s => s.grade === '5').length;
        if(window.gradeChart) window.gradeChart.destroy();
        window.gradeChart = new Chart(document.getElementById('gradePerformanceChart'), { type: 'bar', data: { labels: ['Kutaa 3', 'Kutaa 4', 'Kutaa 5'], datasets: [{ label: 'Number of Students', data: [grade3, grade4, grade5], backgroundColor: '#7c3aed' }] } });
    }

    function updateProgramSummary() {
        const total = students.length;
        const males = students.filter(s => s.sex === 'M').length;
        const females = students.filter(s => s.sex === 'F').length;
        const cwd = students.filter(s => s.specialNeeds !== 'None').length;
        const avgEng = total ? (students.reduce((s, st) => s + st.englishScore, 0) / total) * 20 : 0;
        const avgOromo = total ? (students.reduce((s, st) => s + st.oromoScore, 0) / total) * 20 : 0;
        document.getElementById('programSummary').innerHTML = `<div class="grid-3"><div class="alert alert-info"><strong>📊 Enrollment Summary</strong><br>Total: ${total}<br>Male: ${males}<br>Female: ${females}<br>CWD: ${cwd}</div><div class="alert alert-success"><strong>📈 Average Scores</strong><br>English: ${avgEng.toFixed(1)}%<br>Afaan Oromoo: ${avgOromo.toFixed(1)}%</div><div class="alert alert-info"><strong>🏫 Coverage</strong><br>Woredas: ${[...new Set(woredaData.map(w=>w.woreda))].length}<br>Schools: ${[...new Set(schoolData.map(s=>s.schoolName))].length}</div></div>`;
    }

    function deleteWoreda(id) { if(confirm('Delete woreda data?')) { woredaData = woredaData.filter(w => w.id !== id); saveData(); updateAll(); } }
    function deleteSchool(id) { if(confirm('Delete school data?')) { schoolData = schoolData.filter(s => s.id !== id); saveData(); updateAll(); } }
    function deleteStudent(id) { if(confirm('Delete student?')) { students = students.filter(s => s.id !== id); saveData(); updateAll(); } }

    function generateTaRLReport() {
        const type = document.getElementById('reportType').value;
        const format = document.getElementById('reportFormat').value;
        let html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>TaRL Program Report</title><style>body{font-family:Arial;margin:40px;}h1{color:#2563eb;}table{border-collapse:collapse;width:100%;}th,td{border:1px solid #ddd;padding:8px;}</style></head><body><h1>📊 TaRL Program Report</h1><p>English & Afaan Oromoo | Generated: ${new Date().toLocaleString()}</p>`;
        if(type === 'woreda') {
            html += `<h2>Woreda Level Summary</h2><table><th>Zone</th><th>Woreda</th><th>Schools</th><th>English</th><th>Afaan Oromoo</th>`;
            woredaData.forEach(w => { html += `<tr><td>${w.zone}</td><td>${w.woreda}</td><td>${w.schools}</td><td>${((w.english.total/5)*100).toFixed(0)}%</td><td>${((w.oromo.total/5)*100).toFixed(0)}%</td></tr>`; });
            html += `</table>`;
        } else if(type === 'school') {
            html += `<h2>School Level Summary</h2><tr><th>School</th><th>Grade</th><th>English</th><th>Afaan Oromoo</th>`;
            schoolData.forEach(s => { html += `<tr><td>${s.schoolName}</td><td>Kutaa ${s.grade}</td><td>${((s.english.total/5)*100).toFixed(0)}%</td><td>${((s.oromo.total/5)*100).toFixed(0)}%</td></tr>`; });
            html += `</table>`;
        } else {
            html += `<h2>Student Roster</h2><tr><th>Name</th><th>Grade</th><th>English Score</th><th>English Level</th><th>Afaan Oromoo Score</th><th>Afaan Oromoo Level</th>`;
            students.forEach(s => { html += `<tr><td>${s.name}</td><td>Kutaa ${s.grade}</td><td>${s.englishScore}/5</td><td>${s.englishLevel.text}</td><td>${s.oromoScore}/5</td><td>${s.oromoLevel.text}</td></tr>`; });
            html += `</table>`;
        }
        html += `<h3>📐 Formulas Used</h3><p>English Total = Gulantaa Jalqabaa + Arfii + Jechoota + Keeyyata Salphaa + Seenessaa</p><p>Afaan Oromoo Total = Gulantaa Jalqabaa + Qubee + Jecha + Keeyyata Salphaa + Seenessaa</p></body></html>`;
        if(format === 'html') { const w=window.open(); w.document.write(html); w.document.close(); }
        else { const div=document.createElement('div'); div.innerHTML=html; document.body.appendChild(div); html2pdf().from(div).set({margin:1}).save(); setTimeout(()=>document.body.removeChild(div),1000); }
    }

    function importExcelData(input) {
        const file = input.files[0];
        if(!file) return;
        const reader = new FileReader();
        reader.onload = function(e) {
            const data = new Uint8Array(e.target.result);
            const workbook = XLSX.read(data, { type: 'array' });
            const sheet = workbook.Sheets[workbook.SheetNames[0]];
            const json = XLSX.utils.sheet_to_json(sheet);
            document.getElementById('importPreview').innerHTML = `<div class="alert alert-success">✅ Imported ${json.length} rows.</div>`;
            saveData(); updateAll();
        };
        reader.readAsArrayBuffer(file);
    }

    function loadSampleData() {
        woredaData = [{ id:'WRD1', zone:'East Wolega', woreda:'Leka Dulecha', schools:12, phase:'baseline', grade:'3', totalStudents:420, english:{total:4}, oromo:{total:4} }];
        schoolData = [{ id:'SCH1', zone:'East Wolega', woreda:'Leka Dulecha', schoolId:'001', schoolName:"Ka'umsa Primary School", grade:'3', phase:'baseline', english:{total:4}, oromo:{total:4} }];
        students = [
            { id:'STU1', zone:'East Wolega', woreda:'Leka Dulecha', schoolId:'001', schoolName:"Ka'umsa Primary School", name:'Almaz Tesfaye', sex:'F', grade:'3', specialNeeds:'None', englishScore:4, oromoScore:5, englishLevel:{text:'4_Sentence',class:'level-4'}, oromoLevel:{text:'5_Seenessaa',class:'level-5'} },
            { id:'STU2', zone:'West Wolega', woreda:'Gimbi', schoolId:'002', schoolName:'Chuta Goch Primary School', name:'Biruk Abebe', sex:'M', grade:'4', specialNeeds:'None', englishScore:3, oromoScore:4, englishLevel:{text:'3_Words',class:'level-3'}, oromoLevel:{text:'4_Keeyyata',class:'level-4'} }
        ];
        saveData(); updateAll(); alert('Sample data loaded!');
    }

    function exportAllData() { const a=document.createElement('a'); a.href=URL.createObjectURL(new Blob([JSON.stringify({woredaData,schoolData,students},null,2)],{type:'application/json'})); a.download=`tarl_data.json`; a.click(); }
    function clearAllData() { if(confirm('Delete ALL TaRL data?')) { localStorage.clear(); woredaData=[]; schoolData=[]; students=[]; updateAll(); alert('All data cleared.'); } }
    function escapeHtml(str){ if(!str) return ''; return str.replace(/[&<>]/g, m => m==='&'?'&amp;':m==='<'?'&lt;':'&gt;'); }
    document.querySelectorAll('.nav-btn').forEach(btn=>{ btn.addEventListener('click',()=>{ document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); document.querySelectorAll('.section').forEach(s=>s.classList.remove('active')); document.getElementById(btn.dataset.section).classList.add('active'); }); });
    loadData();
</script>
</body>
</html>
