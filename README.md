<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🧑‍💻 C++ Exam – Functions, Objects & Methods</title>
    <style>
        :root {
            --bg: #f0f4f8;
            --card-bg: #ffffff;
            --primary: #4f6ef7;
            --primary-hover: #3b54e0;
            --success: #22c55e;
            --danger: #ef4444;
            --warning: #f59e0b;
            --text: #1e293b;
            --text-light: #64748b;
            --border: #e2e8f0;
            --radius: 16px;
            --transition: 0.2s ease;
        }
        * { margin:0; padding:0; box-sizing:border-box; }
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background: var(--bg); color: var(--text);
            min-height: 100vh; display: flex; align-items: center; justify-content: center;
            padding: 20px;
        }
        .login-screen, .lock-screen, .exam-container, .warning-overlay {
            background: var(--card-bg);
            border-radius: var(--radius);
            box-shadow: 0 4px 24px rgba(0,0,0,0.08);
        }
        .login-screen { width: 100%; max-width: 500px; padding: 40px; text-align: center; }
        .login-screen h2 { margin-bottom: 20px; }
        .form-group { margin-bottom: 16px; text-align: left; }
        .form-group label { font-weight: 600; display: block; margin-bottom: 4px; }
        .form-group input { width: 100%; padding: 10px; border: 2px solid var(--border); border-radius: 8px; font-size: 1rem; }
        .btn-login {
            padding:14px 28px; border-radius:50px; font-weight:700; font-size:1rem;
            cursor:pointer; border:none; background: var(--primary); color:#fff;
            transition: var(--transition); margin-top: 10px;
        }
        .btn-login:hover { background: var(--primary-hover); }
        .lock-screen { display:none; width: 100%; max-width: 500px; padding: 40px; text-align: center; }
        .lock-screen h2 { margin-bottom: 16px; }
        .lock-screen p { margin-bottom: 24px; color: var(--text-light); }
        .btn-lock {
            padding:14px 28px; border-radius:50px; font-weight:700; font-size:1rem;
            cursor:pointer; border:none; background: var(--primary); color:#fff;
            transition: var(--transition);
        }
        .btn-lock:hover { background: var(--primary-hover); }
        .exam-container { display:none; width:100%; max-width:850px; }
        .warning-overlay {
            position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.85); display:none; align-items:center; justify-content:center;
            z-index:9999; flex-direction: column; color:#fff; text-align:center;
        }
        .warning-overlay h2 { font-size:2rem; margin-bottom:16px; }
        .warning-overlay p { margin-bottom:24px; font-size:1.1rem; }
        .warning-overlay button {
            padding:12px 24px; border-radius:50px; font-weight:700;
            border:none; cursor:pointer; background: var(--warning); color:#000;
        }
        .exam-header {
            background: linear-gradient(135deg, #00599C, #659AD2);
            color: #fff; padding: 22px 28px; display: flex; align-items: center;
            justify-content: space-between; flex-wrap: wrap; gap: 14px;
        }
        .title-area h1 { font-size:1.4rem; font-weight:700; }
        .title-area .subtitle { font-size:0.85rem; opacity:0.9; }
        .timer-box {
            background: rgba(255,255,255,0.18); border-radius: 50px;
            padding: 10px 18px; font-weight: 700; font-size: 1.1rem;
            display: flex; align-items: center; gap: 8px; backdrop-filter: blur(6px);
        }
        .timer-box.state-green { background: #22c55e; }
        .timer-box.state-yellow { background: #f59e0b; animation: pulse 0.8s infinite alternate; }
        .timer-box.state-orange { background: #f97316; animation: pulse 0.6s infinite alternate; }
        .timer-box.state-red { background: #ef4444; animation: pulse 0.5s infinite alternate; }
        @keyframes pulse { from { transform:scale(1); } to { transform:scale(1.06); } }
        .progress-wrap { padding: 14px 28px 8px; }
        .progress-info { display: flex; justify-content: space-between; font-size:0.8rem; color: var(--text-light); margin-bottom:6px; font-weight:600; }
        .progress-bar { height: 8px; background: #e2e8f0; border-radius: 20px; overflow: hidden; }
        .progress-fill { height: 100%; background: var(--primary); border-radius: 20px; width:0%; transition: width 0.4s; }
        .palette { padding: 10px 28px; display: flex; flex-wrap: wrap; gap: 4px; background: #f8fafc; border-bottom: 1px solid var(--border); max-height: 160px; overflow-y: auto; }
        .palette-btn {
            width: 30px; height: 30px; border-radius: 6px; border: 1px solid #cbd5e1;
            background: #fff; font-weight: 600; font-size: 0.75rem; cursor: pointer;
            transition: 0.2s; color: #1e293b;
        }
        .palette-btn.answered { background: #dcfce7; border-color: #22c55e; color: #166534; }
        .palette-btn.current { background: #4f6ef7; color: #fff; border-color: #4f6ef7; }
        .palette-btn.unanswered { background: #f1f5f9; color: #94a3b8; }
        .palette-btn:hover { transform: scale(1.08); }
        .exam-body { padding: 24px 28px 20px; }
        .section-header {
            background: #eef1ff; color: var(--primary); padding: 8px 14px;
            border-radius: 8px; font-weight: 700; margin-bottom: 16px;
            font-size:0.85rem; text-transform: uppercase; letter-spacing: 0.8px;
        }
        .question-text { font-size: 1.15rem; font-weight: 600; margin-bottom: 20px; line-height: 1.5; color: var(--text); }
        .options-list { list-style: none; display: flex; flex-direction: column; gap: 10px; }
        .option-item {
            display: flex; align-items: center; gap: 12px; padding: 12px 14px;
            border: 2px solid var(--border); border-radius: 12px; cursor: pointer;
            transition: var(--transition); background: #fafbfc; font-weight: 500; user-select: none;
        }
        .option-item:hover { border-color:#c4cdf9; background:#f5f7ff; }
        .option-item.selected { border-color: var(--primary); background:#eef1ff; }
        .option-radio {
            width:20px; height:20px; border-radius:50%; border:2px solid #cbd5e1;
            flex-shrink:0; display:flex; align-items:center; justify-content:center; background:#fff;
        }
        .option-item.selected .option-radio { border-color:var(--primary); background:var(--primary); }
        .option-item.selected .option-radio::after { content:''; width:7px; height:7px; background:#fff; border-radius:50%; }
        .option-label { font-weight:600; color:var(--primary); min-width:24px; }
        .option-text { flex:1; color: #1e293b; font-size: 1rem; }
        .tf-group { display:flex; gap:14px; flex-wrap:wrap; }
        .tf-btn {
            flex:1; min-width:120px; padding:16px; border:2px solid var(--border); border-radius:12px;
            text-align:center; cursor:pointer; font-weight:700; background:#fafbfc; user-select:none;
        }
        .tf-btn:hover { border-color:#c4cdf9; background:#f5f7ff; }
        .tf-btn.selected { border-color:var(--primary); background:#eef1ff; }
        .fill-input {
            width:100%; padding:14px 16px; border:2px solid var(--border); border-radius:12px;
            font-size:1rem; font-family:inherit; outline:none; background:#fafbfc;
        }
        .fill-input:focus { border-color:var(--primary); }
        .exam-footer {
            padding:18px 28px 24px; display:flex; justify-content:space-between;
            align-items:center; gap:12px; flex-wrap:wrap; border-top:1px solid var(--border); background:#fdfdfd;
        }
        .btn {
            padding:12px 24px; border-radius:50px; font-weight:700; font-size:0.95rem;
            cursor:pointer; border:none; transition: var(--transition); display:inline-flex; align-items:center; gap:6px;
        }
        .btn-prev { background:#f1f5f9; color:var(--text); }
        .btn-prev:hover { background:#e2e8f0; }
        .btn-next { background:var(--primary); color:#fff; }
        .btn-next:hover { background:var(--primary-hover); }
        .btn-submit { background:var(--success); color:#fff; }
        .btn-submit:hover { background:#16a34a; }
        .btn:disabled { opacity:0.5; pointer-events:none; }
        .review-overlay {
            position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.5); display:flex; align-items:center; justify-content:center; z-index:10;
        }
        .review-card {
            background:#fff; border-radius: var(--radius); padding:28px;
            max-width: 600px; width:90%; max-height:80vh; overflow-y:auto;
        }
        .review-card h3 { margin-bottom:16px; }
        .review-list { display:grid; grid-template-columns: repeat(10,1fr); gap:6px; }
        .review-list button {
            width:32px; height:32px; border-radius:6px; border:1px solid #cbd5e1;
            font-weight:600; cursor:pointer; background:#f1f5f9;
        }
        .review-list button.answered { background:#dcfce7; border-color:#22c55e; }
        .review-list button.unanswered { background:#fef2f2; border-color:#ef4444; }
        .results-screen { text-align:center; padding:40px 28px; }
        .score-circle {
            width:140px; height:140px; border-radius:50%; margin:0 auto 20px;
            display:flex; align-items:center; justify-content:center; font-size:2.6rem; font-weight:800;
            position:relative;
            background: conic-gradient(var(--success) var(--score-percent), #e2e8f0 var(--score-percent));
        }
        .score-circle::before { content:''; position:absolute; inset:12px; background:#fff; border-radius:50%; }
        .score-circle span { position:relative; z-index:1; }
        .stats-grid { display:grid; grid-template-columns: 1fr 1fr; gap:8px; text-align:left; margin:16px 0; }
        .stat-card { background:#f8fafc; border-radius:10px; padding:12px; font-size:0.9rem; border:1px solid var(--border); }
        .email-status { margin-top: 16px; padding: 10px; border-radius: 8px; font-weight:600; }
        .email-status.success { background: #dcfce7; color: #166534; }
        .email-status.fail { background: #fef2f2; color: #991b1b; }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
</head>
<body>
    <div id="loginScreen" class="login-screen">
        <h2>🧑‍💻 Student Details</h2>
        <div class="form-group"><label>Full Name</label><input type="text" id="studentName" placeholder="e.g. John Doe" required></div>
        <div class="form-group"><label>Admission Number</label><input type="text" id="studentAdmission" placeholder="e.g. ADM1234" required></div>
        <div class="form-group"><label>Course</label><input type="text" id="studentCourse" placeholder="e.g. Computer Science" required></div>
        <div class="form-group"><label>Year</label><input type="text" id="studentYear" placeholder="e.g. 2" required></div>
        <div class="form-group"><label>Unit Code</label><input type="text" id="studentUnit" placeholder="e.g. CS101" required></div>
        <div class="form-group"><label>Email (for results copy)</label><input type="email" id="studentEmail" placeholder="student@example.com"></div>
        <button class="btn-login" id="btnLogin">Continue to Exam</button>
    </div>

    <div id="lockScreen" class="lock-screen">
        <h2>🔒 Secure Exam Environment</h2>
        <p>Full‑screen mode required.<br>Do not switch tabs or exit full‑screen.<br>3 violations = auto‑submit.</p>
        <button class="btn-lock" id="btnLock">Lock & Start Exam</button>
    </div>

    <div class="exam-container" id="examContainer">
        <div class="exam-header">
            <div class="title-area">
                <h1>🛠️ C++ Exam – Functions, Objects & Methods</h1>
                <div class="subtitle">100 Questions • 1h30min • Functions, Classes, Constructors, Overloading & more</div>
            </div>
            <div class="timer-box" id="timerBox"><span>⏱</span> <span id="timerDisplay">1:30:00</span></div>
        </div>
        <div class="progress-wrap">
            <div class="progress-info">
                <span id="progressLabel">Question 1 of 100</span>
                <span id="answeredCount">0 answered</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
        </div>
        <div class="palette" id="questionPalette"></div>
        <div class="exam-body" id="examBody"></div>
        <div class="exam-footer" id="examFooter">
            <button class="btn btn-prev" id="btnPrev" disabled>⬅ Previous</button>
            <button class="btn btn-next" id="btnNext">Next ➡</button>
            <button class="btn btn-submit" id="btnSubmit" style="display:none;">✅ Review & Submit</button>
        </div>
        <div class="results-screen" id="resultsScreen" style="display:none;"></div>
    </div>

    <div id="warningOverlay" class="warning-overlay">
        <h2>⚠️ Warning</h2>
        <p>You have left the exam environment.<br><span id="infractionMsg"></span></p>
        <button id="btnReturnFullscreen">Return to Full‑screen</button>
    </div>

    <script>
    window.addEventListener('DOMContentLoaded', function() {
        try {
            // ── TEACHER CONFIGURATION ──
            const TEACHER = {
                email: 'psam3a@gmail.com',
                emailjs: {
                    serviceID: 'service_i22irho',
                    templateID: 'template_77v76hz',
                    publicKey: 'W4nRKEnIwiSMF9B6O'
                },
                webcamEnabled: false
            };

            // ── FULL QUESTIONS POOL (100 items) ──
            const QUESTIONS_POOL = [
                // Section A : MCQ (50)
                { id:1, section:'A', type:'mcq', question:'Which of the following is a valid function declaration?', options:['int add(int a, int b);','function add(int a, int b);','def add(int a, int b);','add(int a, int b);'], answer:0 },
                { id:2, section:'A', type:'mcq', question:'What does the "void" return type mean?', options:['The function returns an integer','The function returns nothing','The function returns a float','The function is a constructor'], answer:1 },
                { id:3, section:'A', type:'mcq', question:'How do you call a function named "display" with no arguments?', options:['display();','call display();','display;','display(void);'], answer:0 },
                { id:4, section:'A', type:'mcq', question:'Which keyword is used to define a function that can be called without an object?', options:['static','friend','inline','virtual'], answer:0 },
                { id:5, section:'A', type:'mcq', question:'What is function overloading?', options:['Using the same function name with different parameters','Defining a function inside another','Calling a function multiple times','Returning multiple values'], answer:0 },
                { id:6, section:'A', type:'mcq', question:'Which parameter passing method allows a function to modify the original variable?', options:['Pass by value','Pass by reference','Pass by pointer','Both B and C'], answer:3 },
                { id:7, section:'A', type:'mcq', question:'What is a default argument?', options:['An argument that must always be provided','An argument that has a predefined value if not supplied','An argument passed automatically by the compiler','The first argument of a function'], answer:1 },
                { id:8, section:'A', type:'mcq', question:'Which statement about inline functions is correct?', options:['They are always expanded inline','They are a request to the compiler for inline expansion','They cannot have loops','They must be defined inside a class'], answer:1 },
                { id:9, section:'A', type:'mcq', question:'What is recursion?', options:['A function calling itself','A loop inside a function','A function that returns a pointer','A function with no parameters'], answer:0 },
                { id:10, section:'A', type:'mcq', question:'Which of the following is a correct class definition?', options:['class MyClass { int x; void show(); };','MyClass class { int x; void show(); };','class MyClass: { int x; };','struct MyClass { int x; void show(); }; (This is also valid, but the question expects class)'], answer:0 },
                { id:11, section:'A', type:'mcq', question:'What is an object?', options:['A variable of a class type','A function inside a class','A pointer to a class','A constant member'], answer:0 },
                { id:12, section:'A', type:'mcq', question:'How do you access a public member function of an object named "obj"?', options:['obj.func();','obj->func();','obj::func();','obj:func();'], answer:0 },
                { id:13, section:'A', type:'mcq', question:'Which access specifier makes members accessible from anywhere?', options:['private','protected','public','static'], answer:2 },
                { id:14, section:'A', type:'mcq', question:'What is a constructor?', options:['A special member function that initializes objects','A function that destroys objects','A regular member function','A static function'], answer:0 },
                { id:15, section:'A', type:'mcq', question:'Which of the following is true about constructors?', options:['They have the same name as the class','They have no return type','They can be overloaded','All of the above'], answer:3 },
                { id:16, section:'A', type:'mcq', question:'What is a destructor?', options:['A function called when an object is created','A function called when an object is destroyed','A function that creates new objects','A static member function'], answer:1 },
                { id:17, section:'A', type:'mcq', question:'How is a destructor named?', options:['~ClassName()','ClassName()','void destructor()','delete()'], answer:0 },
                { id:18, section:'A', type:'mcq', question:'Which of the following is a copy constructor signature?', options:['ClassName(const ClassName &obj);','ClassName(const ClassName obj);','ClassName(ClassName obj);','void copy(ClassName obj);'], answer:0 },
                { id:19, section:'A', type:'mcq', question:'What is a member function?', options:['A function defined outside a class','A function that belongs to a class','A global function','A friend function'], answer:1 },
                { id:20, section:'A', type:'mcq', question:'How can a member function be defined outside the class?', options:['Using the scope resolution operator ::','Using a dot operator','Using a pointer','It cannot be defined outside'], answer:0 },
                { id:21, section:'A', type:'mcq', question:'What is a const member function?', options:['A function that cannot modify the object','A function that returns a constant','A function that takes constant arguments','A function that is static'], answer:0 },
                { id:22, section:'A', type:'mcq', question:'Which keyword is used to define a constant member function?', options:['static','const','virtual','inline'], answer:1 },
                { id:23, section:'A', type:'mcq', question:'What is the "this" pointer?', options:['A pointer to the current object','A pointer to the base class','A global pointer','A pointer to a member function'], answer:0 },
                { id:24, section:'A', type:'mcq', question:'Which of the following is true about static data members?', options:['They are shared among all objects','Each object has its own copy','They cannot be initialized','They are accessed with the dot operator only'], answer:0 },
                { id:25, section:'A', type:'mcq', question:'What is a friend function?', options:['A function that can access private members of a class','A function defined inside a class','A member function of another class','A global function that cannot access private members'], answer:0 },
                { id:26, section:'A', type:'mcq', question:'Which keyword grants access to private members?', options:['public','friend','protected','static'], answer:1 },
                { id:27, section:'A', type:'mcq', question:'What is inheritance?', options:['Creating a new class from an existing class','Overloading functions','Encapsulating data','Polymorphism'], answer:0 },
                { id:28, section:'A', type:'mcq', question:'Which access specifier makes members accessible only in derived classes?', options:['public','private','protected','friend'], answer:2 },
                { id:29, section:'A', type:'mcq', question:'What is a base class?', options:['The class that is being inherited from','The class that inherits','A class without objects','A friend class'], answer:0 },
                { id:30, section:'A', type:'mcq', question:'Which of the following correctly shows public inheritance?', options:['class Derived : public Base { };','class Derived : Base { };','class Derived public Base { };','Derived inherits Base;'], answer:0 },
                { id:31, section:'A', type:'mcq', question:'What is a method?', options:['Another name for a member function','A global function','A static function','A constructor'], answer:0 },
                { id:32, section:'A', type:'mcq', question:'How many destructors can a class have?', options:['One','Two','Multiple','Zero'], answer:0 },
                { id:33, section:'A', type:'mcq', question:'Which of the following is NOT a constructor?', options:['ClassName()','ClassName(int a)','~ClassName()','ClassName(const ClassName &)'], answer:2 },
                { id:34, section:'A', type:'mcq', question:'When is a default constructor called?', options:['When an object is created without arguments','When an object is destroyed','When a copy is made','When a function returns'], answer:0 },
                { id:35, section:'A', type:'mcq', question:'What does the "new" keyword do?', options:['Allocates dynamic memory for an object','Calls the destructor','Creates a static object','Returns a reference'], answer:0 },
                { id:36, section:'A', type:'mcq', question:'Which of the following is true about a static member function?', options:['It can access only static members','It can access non-static members','It must be called with an object','It cannot be defined outside the class'], answer:0 },
                { id:37, section:'A', type:'mcq', question:'How do you initialize a constant member variable in a class?', options:['Using a member initialization list in the constructor','Assigning it inside the constructor body','Using a setter function','It cannot be initialized'], answer:0 },
                { id:38, section:'A', type:'mcq', question:'What is a member initializer list?', options:['A way to initialize data members before the constructor body','A list of member functions','A list of objects','A list of static members'], answer:0 },
                { id:39, section:'A', type:'mcq', question:'Which of the following is a correct way to define a member function outside a class?', options:['int MyClass::getX() { return x; }','int getX() { return x; }','int MyClass.getX() { return x; }','MyClass::int getX() { return x; }'], answer:0 },
                { id:40, section:'A', type:'mcq', question:'What is the output of the following code?  class A { public: int x; A() { x = 10; } }; int main() { A obj; cout << obj.x; }', options:['10','0','Garbage','Error'], answer:0 },
                { id:41, section:'A', type:'mcq', question:'Which of the following is a shallow copy?', options:['Copying the values of data members as they are','Copying only the pointer, not the memory it points to','Creating a new object with same values but separate memory','None of the above'], answer:1 },
                { id:42, section:'A', type:'mcq', question:'Which keyword is used to prevent a class from being inherited?', options:['final','static','const','virtual'], answer:0 },
                { id:43, section:'A', type:'mcq', question:'What is the purpose of the "explicit" keyword?', options:['To prevent implicit conversions by constructors','To make a constructor explicit','To force inline expansion','To define a destructor'], answer:0 },
                { id:44, section:'A', type:'mcq', question:'Which of the following is true about a virtual function?', options:['It must be defined in the base class','It can only be called from a derived class','It is always static','It cannot be overridden'], answer:0 },
                { id:45, section:'A', type:'mcq', question:'What is the difference between a struct and a class in C++?', options:['Struct members are public by default, class members are private','There is no difference','Structs cannot have functions','Classes cannot have constructors'], answer:0 },
                { id:46, section:'A', type:'mcq', question:'How do you declare a reference to an object?', options:['ClassName &ref = obj;','ClassName *ref = &obj;','ClassName ref = obj;','ClassName &ref;'], answer:0 },
                { id:47, section:'A', type:'mcq', question:'Which of the following is an example of a parameterized constructor?', options:['class MyClass { public: MyClass(int a) {} };','class MyClass { public: MyClass() {} };','class MyClass { public: ~MyClass() {} };','class MyClass { public: void MyClass(int a) {} };'], answer:0 },
                { id:48, section:'A', type:'mcq', question:'What is the purpose of the "delete" keyword?', options:['To deallocate memory allocated by new','To call the destructor','To destroy an object','Both A and C'], answer:3 },
                { id:49, section:'A', type:'mcq', question:'Which of the following is a correct way to pass an object to a function by value?', options:['void func(MyClass obj)','void func(MyClass &obj)','void func(const MyClass &obj)','void func(MyClass *obj)'], answer:0 },
                { id:50, section:'A', type:'mcq', question:'What is the benefit of passing objects by const reference?', options:['It avoids copying and prevents modification','It makes the object mutable','It is the only way to pass objects','It forces a deep copy'], answer:0 },

                // Section B : True/False (25)
                { id:51, section:'B', type:'tf', question:'A function can be overloaded based on return type alone.', answer:false },
                { id:52, section:'B', type:'tf', question:'Inline functions guarantee faster execution.', answer:false },
                { id:53, section:'B', type:'tf', question:'A constructor can return a value.', answer:false },
                { id:54, section:'B', type:'tf', question:'A destructor can be overloaded.', answer:false },
                { id:55, section:'B', type:'tf', question:'Static member functions can access non-static data members directly.', answer:false },
                { id:56, section:'B', type:'tf', question:'A friend function is a member of the class.', answer:false },
                { id:57, section:'B', type:'tf', question:'A class can have multiple constructors.', answer:true },
                { id:58, section:'B', type:'tf', question:'The default constructor is called even if you define a parameterized constructor.', answer:false },
                { id:59, section:'B', type:'tf', question:'Objects are passed by value by default.', answer:true },
                { id:60, section:'B', type:'tf', question:'A const member function can modify static data members.', answer:true },
                { id:61, section:'B', type:'tf', question:'The "this" pointer is available only inside non-static member functions.', answer:true },
                { id:62, section:'B', type:'tf', question:'A protected member is accessible from anywhere.', answer:false },
                { id:63, section:'B', type:'tf', question:'Inheritance allows code reusability.', answer:true },
                { id:64, section:'B', type:'tf', question:'A derived class can access private members of its base class directly.', answer:false },
                { id:65, section:'B', type:'tf', question:'The "final" keyword can be applied to a class to prevent inheritance.', answer:true },
                { id:66, section:'B', type:'tf', question:'A copy constructor is called when an object is passed by value.', answer:true },
                { id:67, section:'B', type:'tf', question:'A static data member must be defined outside the class.', answer:true },
                { id:68, section:'B', type:'tf', question:'The "explicit" keyword prevents a constructor from being used for implicit conversions.', answer:true },
                { id:69, section:'B', type:'tf', question:'A member function defined inside the class is automatically inline.', answer:true },
                { id:70, section:'B', type:'tf', question:'A derived class destructor is called before the base class destructor.', answer:true },
                { id:71, section:'B', type:'tf', question:'A static member variable is shared by all objects of the class.', answer:true },
                { id:72, section:'B', type:'tf', question:'A function can return an object.', answer:true },
                { id:73, section:'B', type:'tf', question:'The "delete" operator calls the destructor.', answer:true },
                { id:74, section:'B', type:'tf', question:'A default constructor is provided by the compiler only if no constructors are defined.', answer:true },
                { id:75, section:'B', type:'tf', question:'A class can have multiple destructors.', answer:false },

                // Section C : Short Answer (25)
                { id:76, section:'C', type:'fill', question:'Keyword to define a function that returns nothing.', answer:'void' },
                { id:77, section:'C', type:'fill', question:'Write the syntax to call a function named "run" with no arguments.', answer:'run();' },
                { id:78, section:'C', type:'fill', question:'What is the default parameter passing mechanism in C++?', answer:'pass by value' },
                { id:79, section:'C', type:'fill', question:'Keyword to declare a function that cannot be overridden (C++11).', answer:'final' },
                { id:80, section:'C', type:'fill', question:'What is the name of the special member function that initializes objects?', answer:'constructor' },
                { id:81, section:'C', type:'fill', question:'What symbol is used before the class name in a destructor declaration?', answer:'~' },
                { id:82, section:'C', type:'fill', question:'Keyword to define a static member function.', answer:'static' },
                { id:83, section:'C', type:'fill', question:'What is the scope resolution operator?', answer:'::' },
                { id:84, section:'C', type:'fill', question:'Write the keyword that gives a non-member function access to private members.', answer:'friend' },
                { id:85, section:'C', type:'fill', question:'What is the default access specifier for members of a class?', answer:'private' },
                { id:86, section:'C', type:'fill', question:'What is the default access specifier for members of a struct?', answer:'public' },
                { id:87, section:'C', type:'fill', question:'Keyword to prevent a constructor from implicit conversions.', answer:'explicit' },
                { id:88, section:'C', type:'fill', question:'What is the name of the pointer that refers to the current object inside a member function?', answer:'this' },
                { id:89, section:'C', type:'fill', question:'How many copy constructors can a class have?', answer:'one' },
                { id:90, section:'C', type:'fill', question:'Write the default constructor declaration for a class named "Dog".', answer:'Dog();' },
                { id:91, section:'C', type:'fill', question:'Which keyword is used to create a derived class? (in the declaration)', answer:'public' },
                { id:92, section:'C', type:'fill', question:'What is the term for a function that calls itself?', answer:'recursion' },
                { id:93, section:'C', type:'fill', question:'Write the syntax to define a member function "getX" of class "Point" outside the class.', answer:'int Point::getX()' },
                { id:94, section:'C', type:'fill', question:'What is the default return type of a constructor?', answer:'none' },
                { id:95, section:'C', type:'fill', question:'Which keyword is used to allocate memory for an object dynamically?', answer:'new' },
                { id:96, section:'C', type:'fill', question:'What is the access specifier that allows members to be inherited but not accessible from outside?', answer:'protected' },
                { id:97, section:'C', type:'fill', question:'Write the declaration of a constant member function named "display" that returns void.', answer:'void display() const;' },
                { id:98, section:'C', type:'fill', question:'What is the term for defining multiple functions with the same name but different parameters?', answer:'function overloading' },
                { id:99, section:'C', type:'fill', question:'What is the maximum number of destructors a class can have?', answer:'1' },
                { id:100, section:'C', type:'fill', question:'Write the keyword to define an inline function explicitly.', answer:'inline' }
            ];

            // Randomize within sections, shuffle answer choices
            const shuffleArray = (arr) => {
                for (let i = arr.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [arr[i], arr[j]] = [arr[j], arr[i]];
                }
            };

            const QUESTIONS = [];
            const sectionA = QUESTIONS_POOL.filter(q => q.section === 'A');
            const sectionB = QUESTIONS_POOL.filter(q => q.section === 'B');
            const sectionC = QUESTIONS_POOL.filter(q => q.section === 'C');
            shuffleArray(sectionA); shuffleArray(sectionB); shuffleArray(sectionC);
            let qCounter = 1;
            [sectionA, sectionB, sectionC].forEach(sectionArr => {
                sectionArr.forEach(q => {
                    q.displayId = qCounter++;
                    if (q.type === 'mcq' && q.options) {
                        const correctOption = q.options[q.answer];
                        const indices = q.options.map((_, i) => i);
                        shuffleArray(indices);
                        q.options = indices.map(i => q.options[i]);
                        q.answer = q.options.indexOf(correctOption);
                    }
                    QUESTIONS.push(q);
                });
            });

            const TOTAL_QUESTIONS = QUESTIONS.length;
            const EXAM_TIME = 90 * 60; // 1 hour 30 minutes

            const state = {
                currentIndex: 0,
                answers: new Array(TOTAL_QUESTIONS).fill(null),
                timeRemaining: EXAM_TIME,
                timerInterval: null,
                submitted: false,
                infractions: 0,
                startTime: null,
                student: { name: '', admission: '', course: '', year: '', unit: '', email: '' },
                photoData: null
            };

            const dom = {
                loginScreen: document.getElementById('loginScreen'),
                lockScreen: document.getElementById('lockScreen'),
                examContainer: document.getElementById('examContainer'),
                warningOverlay: document.getElementById('warningOverlay'),
                examBody: document.getElementById('examBody'),
                examFooter: document.getElementById('examFooter'),
                resultsScreen: document.getElementById('resultsScreen'),
                timerDisplay: document.getElementById('timerDisplay'),
                timerBox: document.getElementById('timerBox'),
                progressFill: document.getElementById('progressFill'),
                progressLabel: document.getElementById('progressLabel'),
                answeredCount: document.getElementById('answeredCount'),
                paletteDiv: document.getElementById('questionPalette'),
                btnPrev: document.getElementById('btnPrev'),
                btnNext: document.getElementById('btnNext'),
                btnSubmit: document.getElementById('btnSubmit'),
                btnLock: document.getElementById('btnLock'),
                btnReturnFullscreen: document.getElementById('btnReturnFullscreen'),
                btnLogin: document.getElementById('btnLogin'),
                infractionMsg: document.getElementById('infractionMsg')
            };

            const formatTime = (seconds) => {
                const hrs = Math.floor(seconds / 3600);
                const mins = Math.floor((seconds % 3600) / 60);
                const secs = seconds % 60;
                return hrs > 0 ? `${hrs}:${mins.toString().padStart(2,'0')}:${secs.toString().padStart(2,'0')}` 
                               : `${mins}:${secs.toString().padStart(2,'0')}`;
            };
            const escapeHTML = (str) => String(str).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
            const normalizeAnswer = (str) => String(str).replace(/[<>]/g,'').trim().toLowerCase();
            const isAnswered = (index) => state.answers[index] !== null && state.answers[index] !== '';
            const isAnswerCorrect = (userAns, correct) => {
                if (Array.isArray(correct)) return correct.some(ans => normalizeAnswer(userAns) === normalizeAnswer(ans));
                return normalizeAnswer(userAns) === normalizeAnswer(correct);
            };

            function blockCheating(e) {
                if (state.submitted || dom.lockScreen.style.display !== 'none') return;
                if (['contextmenu','copy','paste','cut','dragstart','selectstart'].includes(e.type)) {
                    e.preventDefault();
                    showWarning();
                }
                if (e.type === 'keydown') {
                    const key = e.key;
                    const ctrl = e.ctrlKey || e.metaKey;
                    if (key === 'F12' || (ctrl && key === 'u') || (ctrl && key === 's') || (ctrl && key === 'p') ||
                        (ctrl && key === 'h') || (ctrl && key === 'j') || (ctrl && key === 'c' && e.shiftKey) ||
                        (ctrl && key === 'i')) {
                        e.preventDefault();
                        showWarning();
                    }
                }
            }

            function requestFullscreen() {
                const docEl = document.documentElement;
                if (docEl.requestFullscreen) docEl.requestFullscreen().catch(()=>{});
                else if (docEl.webkitRequestFullscreen) docEl.webkitRequestFullscreen();
                else if (docEl.msRequestFullscreen) docEl.msRequestFullscreen();
            }
            function isFullscreen() {
                return !!(document.fullscreenElement || document.webkitFullscreenElement || document.msFullscreenElement);
            }
            function showWarning() {
                state.infractions++;
                dom.infractionMsg.textContent = `Violations: ${state.infractions}/3`;
                dom.warningOverlay.style.display = 'flex';
                if (state.infractions >= 3) {
                    alert('Too many violations. Exam terminated.');
                    submitExam();
                }
            }
            function hideWarning() { dom.warningOverlay.style.display = 'none'; }

            function updateTimerUI() {
                dom.timerDisplay.textContent = formatTime(state.timeRemaining);
                dom.timerBox.className = 'timer-box';
                if (state.timeRemaining > 900) dom.timerBox.classList.add('state-green');
                else if (state.timeRemaining > 300) dom.timerBox.classList.add('state-yellow');
                else if (state.timeRemaining > 120) dom.timerBox.classList.add('state-orange');
                else dom.timerBox.classList.add('state-red');
            }
            function startTimer() {
                state.startTime = new Date();
                updateTimerUI();
                state.timerInterval = setInterval(() => {
                    state.timeRemaining--;
                    updateTimerUI();
                    saveProgress();
                    if (state.timeRemaining <= 0) {
                        clearInterval(state.timerInterval);
                        alert('⏰ Time is up! Your exam will be submitted automatically.');
                        submitExam();
                    }
                }, 1000);
            }

            function renderPalette() {
                let html = '';
                for (let i = 0; i < TOTAL_QUESTIONS; i++) {
                    let cls = 'palette-btn';
                    if (i === state.currentIndex) cls += ' current';
                    else if (isAnswered(i)) cls += ' answered';
                    else cls += ' unanswered';
                    html += `<button class="${cls}" data-index="${i}">${i+1}</button>`;
                }
                dom.paletteDiv.innerHTML = html;
                document.querySelectorAll('.palette-btn').forEach(btn => {
                    btn.addEventListener('click', () => goToQuestion(parseInt(btn.dataset.index)));
                });
            }
            function updateProgressUI() {
                const answered = state.answers.filter(a => a !== null && a !== '').length;
                dom.progressFill.style.width = `${(answered / TOTAL_QUESTIONS) * 100}%`;
                dom.progressLabel.textContent = `Question ${state.currentIndex + 1} of ${TOTAL_QUESTIONS}`;
                dom.answeredCount.textContent = `${answered} answered`;
                renderPalette();
            }

            function saveProgress() {
                localStorage.setItem('cppExamFunctionsProgress', JSON.stringify({
                    answers: state.answers,
                    timeRemaining: state.timeRemaining,
                    currentIndex: state.currentIndex,
                    infractions: state.infractions,
                    student: state.student,
                    startTime: state.startTime ? state.startTime.toISOString() : null
                }));
            }
            function loadProgress() {
                const saved = localStorage.getItem('cppExamFunctionsProgress');
                if (!saved) return false;
                try {
                    const data = JSON.parse(saved);
                    if (data.answers && data.timeRemaining !== undefined) {
                        state.answers = data.answers;
                        state.timeRemaining = data.timeRemaining;
                        state.currentIndex = data.currentIndex || 0;
                        state.infractions = data.infractions || 0;
                        state.student = data.student || {};
                        state.startTime = data.startTime ? new Date(data.startTime) : null;
                        return true;
                    }
                } catch(e) {}
                return false;
            }
            function clearProgress() { localStorage.removeItem('cppExamFunctionsProgress'); }

            function goToQuestion(index) {
                if (index < 0 || index >= TOTAL_QUESTIONS) return;
                state.currentIndex = index;
                renderQuestion();
            }
            function updateButtons() {
                dom.btnPrev.disabled = (state.currentIndex === 0);
                if (state.currentIndex === TOTAL_QUESTIONS - 1) {
                    dom.btnNext.style.display = 'none';
                    dom.btnSubmit.style.display = 'inline-flex';
                } else {
                    dom.btnNext.style.display = 'inline-flex';
                    dom.btnSubmit.style.display = 'none';
                    dom.btnNext.disabled = false;
                }
            }
            function saveAnswer(index, value) {
                state.answers[index] = value;
                updateProgressUI();
                saveProgress();
            }

            function renderQuestion() {
                const q = QUESTIONS[state.currentIndex];
                const saved = state.answers[state.currentIndex];
                let html = '';
                const sectionNames = { A:'Multiple Choice (1-50)', B:'True / False (51-75)', C:'Short Answer (76-100)' };
                html += `<div class="section-header">Section ${q.section}: ${sectionNames[q.section]}</div>`;
                html += `<div class="question-text">${q.displayId}. ${escapeHTML(q.question)}</div>`;

                if (q.type === 'mcq') {
                    html += '<ul class="options-list">';
                    const labels = ['A','B','C','D'];
                    q.options.forEach((opt, i) => {
                        const selected = (saved === i) ? ' selected' : '';
                        html += `<li class="option-item${selected}" data-opt-index="${i}"><div class="option-radio"></div><span class="option-label">${labels[i]}.</span><span class="option-text">${escapeHTML(opt)}</span></li>`;
                    });
                    html += '</ul>';
                } else if (q.type === 'tf') {
                    html += '<div class="tf-group">';
                    const opts = [{val:true, label:'✅ True'}, {val:false, label:'❌ False'}];
                    opts.forEach(opt => {
                        const selected = (saved === opt.val) ? ' selected' : '';
                        html += `<div class="tf-btn${selected}" data-tf-val="${opt.val}">${opt.label}</div>`;
                    });
                    html += '</div>';
                } else if (q.type === 'fill') {
                    const val = saved !== null ? saved : '';
                    html += `<input type="text" class="fill-input" id="fillInput" placeholder="Type your answer..." value="${escapeHTML(val)}" autocomplete="off">`;
                }
                dom.examBody.innerHTML = html;
                attachQuestionListeners();
                updateButtons();
                updateProgressUI();
                saveProgress();
            }

            function attachQuestionListeners() {
                const q = QUESTIONS[state.currentIndex];
                if (q.type === 'mcq') {
                    const items = dom.examBody.querySelectorAll('.option-item');
                    items.forEach(item => {
                        item.addEventListener('click', function() {
                            if (state.submitted) return;
                            const idx = parseInt(this.dataset.optIndex);
                            items.forEach(el => el.classList.remove('selected'));
                            this.classList.add('selected');
                            saveAnswer(state.currentIndex, idx);
                        });
                    });
                } else if (q.type === 'tf') {
                    const btns = dom.examBody.querySelectorAll('.tf-btn');
                    btns.forEach(btn => {
                        btn.addEventListener('click', function() {
                            if (state.submitted) return;
                            const val = this.dataset.tfVal === 'true';
                            btns.forEach(el => el.classList.remove('selected'));
                            this.classList.add('selected');
                            saveAnswer(state.currentIndex, val);
                        });
                    });
                } else if (q.type === 'fill') {
                    const input = dom.examBody.querySelector('#fillInput');
                    if (input) {
                        input.addEventListener('input', function() {
                            if (state.submitted) return;
                            saveAnswer(state.currentIndex, this.value.trim());
                        });
                        setTimeout(() => input.focus(), 100);
                    }
                }
            }

            function showReviewScreen() {
                const answered = state.answers.filter(a => a !== null && a !== '').length;
                const unanswered = TOTAL_QUESTIONS - answered;
                const overlay = document.createElement('div');
                overlay.className = 'review-overlay';
                overlay.innerHTML = `
                    <div class="review-card">
                        <h3>📋 Review Your Answers</h3>
                        <p>Answered: <strong>${answered}</strong> | Unanswered: <strong style="color:#ef4444;">${unanswered}</strong></p>
                        <div class="review-list" id="reviewGrid"></div>
                        <div style="margin-top:20px; display:flex; gap:10px; justify-content:flex-end;">
                            <button class="btn btn-prev" id="reviewCancel">🔙 Continue Exam</button>
                            <button class="btn btn-submit" id="reviewConfirm">✅ Submit Final</button>
                        </div>
                    </div>`;
                document.body.appendChild(overlay);
                const grid = overlay.querySelector('#reviewGrid');
                for (let i = 0; i < TOTAL_QUESTIONS; i++) {
                    const btn = document.createElement('button');
                    btn.textContent = i+1;
                    btn.className = isAnswered(i) ? 'answered' : 'unanswered';
                    btn.addEventListener('click', () => {
                        document.body.removeChild(overlay);
                        goToQuestion(i);
                    });
                    grid.appendChild(btn);
                }
                overlay.querySelector('#reviewCancel').addEventListener('click', () => {
                    document.body.removeChild(overlay);
                });
                overlay.querySelector('#reviewConfirm').addEventListener('click', () => {
                    document.body.removeChild(overlay);
                    submitExam();
                });
            }

            function calculateScore() {
                let correct = 0;
                const details = [];
                QUESTIONS.forEach((q, i) => {
                    const userAns = state.answers[i];
                    let isCorrect = false;
                    if (q.type === 'mcq') isCorrect = (userAns === q.answer);
                    else if (q.type === 'tf') isCorrect = (userAns === q.answer);
                    else if (q.type === 'fill') {
                        if (userAns === null || userAns === '') isCorrect = false;
                        else isCorrect = isAnswerCorrect(userAns, q.answer);
                    }
                    if (isCorrect) correct++;
                    details.push({ question:q, userAnswer:userAns, isCorrect });
                });
                return { correct, total: TOTAL_QUESTIONS, details };
            }

            function getGrade(percent) {
                if (percent >= 90) return 'A+'; if (percent >= 80) return 'A';
                if (percent >= 70) return 'B'; if (percent >= 60) return 'C';
                if (percent >= 50) return 'D'; return 'F';
            }

            function generateResultsText(scoreData) {
                const { correct, details } = scoreData;
                const percent = Math.round((correct / TOTAL_QUESTIONS) * 100);
                const grade = getGrade(percent);
                const timeUsed = EXAM_TIME - state.timeRemaining;
                const attempted = state.answers.filter(a => a !== null && a !== '').length;
                const skipped = TOTAL_QUESTIONS - attempted;
                const wrong = attempted - correct;
                const sections = { A:{total:0, correct:0}, B:{total:0, correct:0}, C:{total:0, correct:0} };
                details.forEach((d,i) => {
                    const sec = QUESTIONS[i].section;
                    sections[sec].total++;
                    if (d.isCorrect) sections[sec].correct++;
                });
                let text = `Student: ${state.student.name} | Admission: ${state.student.admission}\nCourse: ${state.student.course} | Year: ${state.student.year} | Unit: ${state.student.unit}\n`;
                text += `Email: ${state.student.email}\nStarted: ${state.startTime?.toLocaleTimeString() || 'N/A'}\n`;
                text += `Finished: ${new Date().toLocaleTimeString()}\nDuration: ${formatTime(timeUsed)}\n`;
                text += `Browser: ${navigator.userAgent}\nPlatform: ${navigator.platform}\nInfractions: ${state.infractions}\n`;
                text += `Score: ${percent}% (${correct}/${TOTAL_QUESTIONS}) | Grade: ${grade}\n`;
                text += `Attempted: ${attempted} | Correct: ${correct} | Wrong: ${wrong} | Skipped: ${skipped}\n`;
                text += `Section A: ${sections.A.correct}/${sections.A.total} | B: ${sections.B.correct}/${sections.B.total} | C: ${sections.C.correct}/${sections.C.total}\n\nAnswer Breakdown:\n`;
                details.forEach((d, i) => {
                    const q = d.question;
                    const correctAns = q.type === 'mcq' ? q.options[q.answer] : (q.type === 'tf' ? (q.answer ? 'True' : 'False') : (Array.isArray(q.answer) ? q.answer.join(' / ') : q.answer));
                    const userAnsText = d.userAnswer !== null && d.userAnswer !== '' ? (q.type === 'mcq' ? q.options[d.userAnswer] : (q.type === 'tf' ? (d.userAnswer ? 'True' : 'False') : d.userAnswer)) : 'No answer';
                    text += `Q${q.displayId}. ${q.question}\n   Student: ${userAnsText}\n   Correct: ${correctAns} ${d.isCorrect ? '✅' : '❌'}\n\n`;
                });
                return text;
            }

            // Initialize EmailJS
            if (typeof emailjs !== 'undefined') {
                emailjs.init(TEACHER.emailjs.publicKey);
            } else {
                console.warn('EmailJS library not loaded');
            }

            async function sendEmail(scoreData) {
                const statusDiv = document.getElementById('emailStatus');
                if (!statusDiv) return;
                statusDiv.textContent = '📤 Sending results...';
                statusDiv.className = 'email-status';
                try {
                    await emailjs.send(TEACHER.emailjs.serviceID, TEACHER.emailjs.templateID, {
                        student_name: state.student.name,
                        admission: state.student.admission,
                        course: state.student.course,
                        year: state.student.year,
                        unit: state.student.unit,
                        student_email: state.student.email,
                        score: Math.round((scoreData.correct / TOTAL_QUESTIONS) * 100),
                        grade: getGrade(Math.round((scoreData.correct / TOTAL_QUESTIONS) * 100)),
                        correct: scoreData.correct,
                        wrong: (state.answers.filter(a => a !== null && a !== '').length - scoreData.correct),
                        attempted: state.answers.filter(a => a !== null && a !== '').length,
                        skipped: TOTAL_QUESTIONS - state.answers.filter(a => a !== null && a !== '').length,
                        time: formatTime(EXAM_TIME - state.timeRemaining),
                        infractions: state.infractions,
                        results: generateResultsText(scoreData),
                        browser: navigator.userAgent,
                        platform: navigator.platform,
                        language: navigator.language,
                        start_time: state.startTime ? state.startTime.toLocaleTimeString() : '',
                        end_time: new Date().toLocaleTimeString()
                    });
                    statusDiv.textContent = '✅ Results sent successfully.';
                    statusDiv.className = 'email-status success';
                } catch(e) {
                    statusDiv.textContent = '❌ Failed to send results.';
                    statusDiv.className = 'email-status fail';
                }
            }

            async function submitExam() {
                if (state.submitted) return;
                state.submitted = true;
                clearInterval(state.timerInterval);
                clearProgress();
                if (document.fullscreenElement) document.exitFullscreen();
                const scoreData = calculateScore();
                renderResults(scoreData);
                sendEmail(scoreData);
                dom.timerBox.classList.remove('state-yellow','state-orange','state-red');
            }

            function renderResults(scoreData) {
                const { correct } = scoreData;
                const percent = Math.round((correct / TOTAL_QUESTIONS) * 100);
                const grade = getGrade(percent);
                const timeUsed = EXAM_TIME - state.timeRemaining;
                const attempted = state.answers.filter(a => a !== null && a !== '').length;
                const skipped = TOTAL_QUESTIONS - attempted;
                const wrong = attempted - correct;
                let html = `<span class="emoji-big">${percent>=90?'🏆':percent>=70?'😊':percent>=50?'🤔':'📖'}</span>`;
                html += `<div class="score-circle" style="--score-percent:${percent}%;"><span>${percent}%</span></div>`;
                html += `<h2>Grade: ${grade}</h2>`;
                html += `<p>You scored ${correct} out of ${TOTAL_QUESTIONS} (${percent}%)</p>`;
                html += `<div class="stats-grid">
                    <div class="stat-card">⏱ Time used: ${formatTime(timeUsed)}</div>
                    <div class="stat-card">📝 Attempted: ${attempted}</div>
                    <div class="stat-card">✅ Correct: ${correct}</div>
                    <div class="stat-card">❌ Wrong: ${wrong}</div>
                    <div class="stat-card">⏭ Skipped: ${skipped}</div>
                    <div class="stat-card">📊 Accuracy: ${attempted ? Math.round((correct/attempted)*100) : 0}%</div>
                </div>`;
                html += `<p style="color:#ef4444;">Infractions: ${state.infractions}</p>`;
                html += `<div id="emailStatus" class="email-status"></div>`;
                html += `<button class="btn btn-outline" onclick="location.reload()" style="margin-top:20px;">🔄 Retake Exam</button>`;
                dom.resultsScreen.innerHTML = html;
                dom.examBody.style.display = 'none';
                dom.examFooter.style.display = 'none';
                document.querySelector('.progress-wrap').style.display = 'none';
                dom.paletteDiv.style.display = 'none';
                dom.resultsScreen.style.display = 'block';
            }

            function initExam() {
                const resumed = loadProgress();
                if (!resumed || state.timeRemaining <= 0 || state.submitted) {
                    state.answers = new Array(TOTAL_QUESTIONS).fill(null);
                    state.timeRemaining = EXAM_TIME;
                    state.currentIndex = 0;
                    state.infractions = 0;
                    clearProgress();
                }
                state.submitted = false;
                dom.examBody.style.display = 'block';
                dom.examFooter.style.display = 'flex';
                dom.resultsScreen.style.display = 'none';
                document.querySelector('.progress-wrap').style.display = 'block';
                dom.paletteDiv.style.display = 'flex';
                renderQuestion();
                startTimer();
                updateProgressUI();
            }

            // Event listeners
            dom.btnLogin.addEventListener('click', () => {
                const name = document.getElementById('studentName').value.trim();
                const admission = document.getElementById('studentAdmission').value.trim();
                const course = document.getElementById('studentCourse').value.trim();
                const year = document.getElementById('studentYear').value.trim();
                const unit = document.getElementById('studentUnit').value.trim();
                const email = document.getElementById('studentEmail').value.trim();
                if (!name || !admission || !course || !year || !unit) {
                    alert('Please fill all required fields.');
                    return;
                }
                state.student = { name, admission, course, year, unit, email };
                dom.loginScreen.style.display = 'none';
                dom.lockScreen.style.display = 'block';
                saveProgress();
            });

            dom.btnLock.addEventListener('click', () => {
                requestFullscreen();
                dom.lockScreen.style.display = 'none';
                dom.examContainer.style.display = 'block';
                initExam();
            });

            dom.btnReturnFullscreen.addEventListener('click', () => {
                requestFullscreen();
                hideWarning();
            });

            dom.btnPrev.addEventListener('click', () => {
                if (state.currentIndex > 0) goToQuestion(state.currentIndex - 1);
            });
            dom.btnNext.addEventListener('click', () => {
                if (state.currentIndex < TOTAL_QUESTIONS - 1) goToQuestion(state.currentIndex + 1);
            });
            dom.btnSubmit.addEventListener('click', showReviewScreen);

            ['contextmenu','copy','paste','cut','dragstart','selectstart'].forEach(ev => {
                document.addEventListener(ev, blockCheating);
            });
            document.addEventListener('keydown', blockCheating);
            document.addEventListener('fullscreenchange', () => {
                if (!isFullscreen() && !state.submitted && dom.lockScreen.style.display === 'none') showWarning();
                else if (isFullscreen() && dom.warningOverlay.style.display === 'flex') hideWarning();
            });
            document.addEventListener('visibilitychange', () => {
                if (document.hidden && !state.submitted && dom.lockScreen.style.display === 'none') showWarning();
            });
            window.addEventListener('beforeunload', (e) => {
                if (!state.submitted && dom.lockScreen.style.display === 'none') {
                    e.preventDefault();
                    e.returnValue = '';
                }
            });

            // Keyboard shortcuts for answers
            document.addEventListener('keydown', (e) => {
                if (state.submitted || dom.lockScreen.style.display !== 'none') return;
                if (e.key === 'ArrowLeft' && state.currentIndex > 0) { e.preventDefault(); goToQuestion(state.currentIndex-1); }
                else if (e.key === 'ArrowRight' && state.currentIndex < TOTAL_QUESTIONS-1) { e.preventDefault(); goToQuestion(state.currentIndex+1); }
                else if (e.key === 'Enter' && state.currentIndex === TOTAL_QUESTIONS-1) { e.preventDefault(); showReviewScreen(); }
                const q = QUESTIONS[state.currentIndex];
                if (q.type === 'mcq') {
                    const keyMap = { 'a':0,'b':1,'c':2,'d':3,'1':0,'2':1,'3':2,'4':3 };
                    const idx = keyMap[e.key.toLowerCase()];
                    if (idx !== undefined && idx < q.options.length) {
                        e.preventDefault();
                        saveAnswer(state.currentIndex, idx);
                        const items = dom.examBody.querySelectorAll('.option-item');
                        items.forEach(el => el.classList.remove('selected'));
                        if (items[idx]) items[idx].classList.add('selected');
                    }
                }
                if (q.type === 'tf') {
                    if (e.key.toLowerCase() === 't' || e.key === '1') {
                        e.preventDefault();
                        saveAnswer(state.currentIndex, true);
                        const btns = dom.examBody.querySelectorAll('.tf-btn');
                        btns.forEach(el => el.classList.remove('selected'));
                        if (btns[0]) btns[0].classList.add('selected');
                    } else if (e.key.toLowerCase() === 'f' || e.key === '2') {
                        e.preventDefault();
                        saveAnswer(state.currentIndex, false);
                        const btns = dom.examBody.querySelectorAll('.tf-btn');
                        btns.forEach(el => el.classList.remove('selected'));
                        if (btns[1]) btns[1].classList.add('selected');
                    }
                }
            });

            console.log('✅ Functions, Objects & Methods exam loaded successfully.');
        } catch (error) {
            alert('❌ A problem occurred while setting up the exam. Please copy this error:\n\n' + error.message + '\n\nStack: ' + error.stack);
            console.error(error);
        }
    });
    </script>
</body>
</html>
