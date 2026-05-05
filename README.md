# esd  python3 - << 'PYEOF'
import json
import random

# --- 1. CDAC CCE Exam Question Bank ---
# m = module (0: Embedded C, 1: DSA, 2: Operating Systems, 3: Microcontrollers)
# q = question, o = options list, a = correct index, e = explanation
raw_questions = [
    # Module 0: Embedded C [cite: 66, 106]
    {
        "m": 0, "q": "What is the output? int a=10,b=20; printf(\"%d\",a+++b);",
        "o": ["30", "31", "Error", "29"], "a": 0,
        "e": "a+++ is parsed as (a++)+b. Post-increment returns the current value (10). So 10+20=30 is printed."
    },
    {
        "m": 0, "q": "Which keyword is used to prevent the compiler from optimizing a variable that can be changed by hardware?",
        "o": ["static", "register", "volatile", "extern"], "a": 2,
        "e": "The volatile keyword tells the compiler that the variable's value can change at any time without any action being taken by the code, such as by a hardware peripheral or an ISR. [cite: 111]"
    },
    {
        "m": 0, "q": "How do you set the 4th bit of a register REG?",
        "o": ["REG &= ~(1 << 4);", "REG |= (1 << 4);", "REG ^= (1 << 4);", "REG >>= 4;"], "a": 1,
        "e": "Using the bitwise OR (|) with a shifted mask (1 << 4) sets only the 4th bit (0-indexed) to 1 without affecting others. [cite: 117]"
    },
    {
        "m": 0, "q": "Which function is used to allocate memory that is automatically initialized to zero?",
        "o": ["malloc()", "realloc()", "calloc()", "free()"], "a": 2,
        "e": "Unlike malloc, calloc initializes the allocated memory block to zero. [cite: 119]"
    },
    
    # Module 1: DSA [cite: 108, 120]
    {
        "m": 1, "q": "What is the time complexity of searching for an element in a balanced Binary Search Tree (BST)?",
        "o": ["O(1)", "O(n)", "O(log n)", "O(n log n)"], "a": 2,
        "e": "In a balanced tree, each step eliminates half of the remaining nodes, leading to logarithmic time complexity. [cite: 122]"
    },
    {
        "m": 1, "q": "Which data structure is typically used to implement a LIFO mechanism?",
        "o": ["Queue", "Stack", "Linked List", "Heap"], "a": 1,
        "e": "Stacks operate on the LIFO principle, where the last element added is the first one removed. [cite: 124]"
    },
    {
        "m": 1, "q": "What is the result of the post-order traversal of a tree with root 'A', left child 'B', and right child 'C'?",
        "o": ["A, B, C", "B, C, A", "B, A, C", "C, B, A"], "a": 1,
        "e": "Post-order traversal visits nodes in the order: Left → Right → Root. [cite: 126]"
    },

    # Module 2: Operating Systems [cite: 88, 127]
    {
        "m": 2, "q": "Which part of the OS communicates directly with the computer hardware?",
        "o": ["Shell", "GUI", "Kernel", "Compiler"], "a": 2,
        "e": "The Kernel is the core of the OS that manages system resources and hardware communication. [cite: 129, 146]"
    },
    {
        "m": 2, "q": "What is a 'deadlock' in an operating system?",
        "o": ["A process is running too fast.", "Two or more processes are waiting indefinitely for each other to release resources.", "The system has run out of memory.", "The computer is turned off."], "a": 1,
        "e": "Deadlock occurs when processes are stuck in a circular wait for resources held by one another. [cite: 133]"
    },
    {
        "m": 2, "q": "What causes 'thrashing' in virtual memory management?",
        "o": ["CPU overheating", "OS spending more time swapping pages than executing programs", "Disk defragmentation", "Too many background threads"], "a": 1,
        "e": "Thrashing occurs when the OS spends more time moving data back and forth (paging) than actually running programs. [cite: 160]"
    },

    # Module 3: Microcontrollers [cite: 31, 134]
    {
        "m": 3, "q": "Which communication protocol uses only two wires (SDA and SCL)?",
        "o": ["SPI", "UART", "I2C", "CAN"], "a": 2,
        "e": "I2C (Inter-Integrated Circuit) uses a Serial Data (SDA) line and a Serial Clock (SCL) line. [cite: 136]"
    },
    {
        "m": 3, "q": "What is the primary purpose of a Watchdog Timer?",
        "o": ["To speed up the CPU", "To reset the microcontroller if the software hangs", "To keep track of RTC", "To manage external interrupts"], "a": 1,
        "e": "A Watchdog Timer is a hardware timer that triggers a system reset if the main program fails to 'kick' or reset it within a specific timeframe. [cite: 139]"
    },
    {
        "m": 3, "q": "In the ARM Cortex-M architecture, which register holds the address of the next instruction to be executed?",
        "o": ["Stack Pointer (SP)", "Link Register (LR)", "Program Counter (PC)", "Status Register (PSR)"], "a": 2,
        "e": "The Program Counter (PC) always points to the memory address of the instruction currently being fetched/executed. [cite: 141]"
    }
]

# --- 2. Shuffle Options Logic ---
shuffled_questions = []
for q in raw_questions:
    opts = list(q['o'])
    correct_text = opts[q['a']]
    random.shuffle(opts)
    new_a = opts.index(correct_text)
    shuffled_questions.append({
        "m": q["m"],
        "q": q["q"],
        "o": opts,
        "a": new_a,
        "e": q["e"]
    })

# --- 3. HTML DOM & JavaScript App ---
html_content = f'''
    <div id="s-land" class="screen active">
        <div class="eyebrow"><div class="edot"></div><span class="etxt">PGCP-ESD Feb 2026</span></div>
        <h1 class="lh1">CDAC CCE <br/><span class="g">MOCK EXAM</span></h1>
        <p class="lp">Comprehensive evaluation covering Embedded C, Data Structures, Operating Systems, and Microcontroller Interfacing.</p>
        
        <div class="ebox">
            <h3>Candidate Registration</h3>
            <div class="fld">
                <label>Full Name</label>
                <input type="text" id="cand-name" placeholder="Enter your name to begin..." autocomplete="off"/>
            </div>
            <button class="bgo" onclick="startExam()">COMMENCE EXAM</button>
            <div class="lbopen">
                <button onclick="showLeaderboard()">View Global Leaderboard</button>
            </div>
        </div>
    </div>

    <div id="s-exam" class="screen">
        <div class="topbar">
            <div class="tbl">
                <div class="tblogo">CDAC MOCK</div>
                <div class="tbmod" id="mod-indicator">Embedded C</div>
            </div>
            <div class="tbc timer"><span class="tv" id="timer-val">45:00</span></div>
            <div class="tbr">
                <div class="lsc">Score: <b id="score-val">0</b></div>
                <button class="bsub" onclick="submitExam()">FINISH EXAM</button>
            </div>
        </div>
        
        <div class="exambody">
            <div class="qpanel">
                <div class="qhead">
                    <div class="qmeta">
                        <div class="qnb" id="q-num">Q.1</div>
                        <div class="qmk">+1 Mark</div>
                    </div>
                </div>
                
                <div class="qtbox">
                    <div class="qtxt" id="q-text">Loading question...</div>
                </div>
                
                <div class="owrap" id="opts-wrap">
                    </div>

                <div class="expbox" id="exp-box">
                    <div class="exphd">
                        <div class="expttl">Explanation</div>
                        <div class="expres" id="exp-res"></div>
                    </div>
                    <div class="expbody" id="exp-text"></div>
                </div>

                <div class="qnav">
                    <button class="bnav" id="btn-prev" onclick="nav(-1)">Previous</button>
                    <div class="qprg" id="q-prog">1 / 13</div>
                    <button class="bnav nxt" id="btn-next" onclick="nav(1)">Next Question</button>
                </div>
            </div>
        </div>
    </div>

    <div id="s-result" class="screen">
        <div class="rw">
            <div class="rhero">
                <div class="re" id="r-emoji">🎯</div>
                <div class="rn" id="r-name">Candidate</div>
                <div class="rsc" id="r-score">0<span class="fr">/13</span></div>
                <div class="rmsg" id="r-msg">Evaluation Complete.</div>
            </div>
            <div class="racts">
                <button class="rab se" onclick="location.reload()">Return Home</button>
                <button class="rab gr" onclick="showLeaderboard()">View Leaderboard</button>
            </div>
        </div>
    </div>

    <div id="s-lb" class="screen">
        <div class="lbw">
            <div class="lbhd">
                <h2>Top Candidates</h2>
                <div class="lbbtns">
                    <button class="lbbtn" onclick="location.reload()">Back</button>
                </div>
            </div>
            <div class="lblist" id="lb-list">
                </div>
        </div>
    </div>

    <script>
        const db = {json.dumps(shuffled_questions)};
        const modules = ["Embedded C", "Data Structures", "Operating Systems", "Microcontrollers"];
        let currentQ = 0;
        let score = 0;
        let candidateName = "";
        let timerInt;
        let timeLeft = 2700; // 45 mins
        let answered = new Array(db.length).fill(false);

        function showScreen(id) {{
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }}

        function startExam() {{
            const nameInput = document.getElementById('cand-name').value.trim();
            if(!nameInput) return alert("Please enter your name to begin.");
            candidateName = nameInput;
            showScreen('s-exam');
            loadQ();
            
            // Start Timer
            timerInt = setInterval(() => {{
                timeLeft--;
                let m = Math.floor(timeLeft / 60);
                let s = timeLeft % 60;
                document.getElementById('timer-val').innerText = `${{m < 10 ? '0'+m : m}}:${{s < 10 ? '0'+s : s}}`;
                if(timeLeft <= 0) submitExam();
            }}, 1000);
        }}

        function loadQ() {{
            const q = db[currentQ];
            document.getElementById('mod-indicator').innerText = modules[q.m];
            document.getElementById('q-num').innerText = `Q.${{currentQ + 1}}`;
            document.getElementById('q-text').innerText = q.q;
            document.getElementById('q-prog').innerText = `${{currentQ + 1}} / ${{db.length}}`;
            
            const owrap = document.getElementById('opts-wrap');
            owrap.innerHTML = '';
            
            // Hide explanation initially
            const expBox = document.getElementById('exp-box');
            expBox.classList.remove('open');

            q.o.forEach((opt, idx) => {{
                const btn = document.createElement('div');
                btn.className = `obt ${{answered[currentQ] ? 'dis' : ''}}`;
                btn.innerHTML = `<div class="olbl">${{String.fromCharCode(65 + idx)}}</div><div>${{opt}}</div>`;
                
                if(answered[currentQ]) {{
                    if(idx === q.a) btn.classList.add('cor');
                }} else {{
                    btn.onclick = () => handleAnswer(idx, btn);
                }}
                owrap.appendChild(btn);
            }});

            if(answered[currentQ]) showExplanation();
            
            document.getElementById('btn-prev').disabled = currentQ === 0;
            document.getElementById('btn-next').innerText = currentQ === db.length - 1 ? "Finish" : "Next Question";
        }}

        function handleAnswer(idx, btnNode) {{
            if(answered[currentQ]) return;
            const q = db[currentQ];
            answered[currentQ] = true;
            
            const isCorrect = (idx === q.a);
            if(isCorrect) {{
                score++;
                document.getElementById('score-val').innerText = score;
                btnNode.classList.add('cor');
            }} else {{
                btnNode.classList.add('wrg');
                document.querySelectorAll('.obt')[q.a].classList.add('cor');
            }}
            
            showExplanation(isCorrect);
        }}

        function showExplanation(isCorrect) {{
            const q = db[currentQ];
            const expBox = document.getElementById('exp-box');
            const expRes = document.getElementById('exp-res');
            
            expBox.classList.add('open');
            document.getElementById('exp-text').innerText = q.e;
            
            if(isCorrect !== undefined) {{
                expRes.className = `expres ${{isCorrect ? 'ok' : 'ng'}}`;
                expRes.innerText = isCorrect ? 'Correct +1' : 'Incorrect 0';
            }} else {{
                expRes.className = 'expres';
                expRes.innerText = 'Reviewed';
            }}
        }}

        function nav(dir) {{
            if(dir === 1 && currentQ === db.length - 1) return submitExam();
            currentQ += dir;
            loadQ();
        }}

        function submitExam() {{
            clearInterval(timerInt);
            saveToLeaderboard();
            
            document.getElementById('r-name').innerText = candidateName;
            document.getElementById('r-score').innerHTML = `${{score}}<span class="fr">/${{db.length}}</span>`;
            
            let pct = (score / db.length) * 100;
            let msg = pct > 80 ? "Excellent understanding of CDAC modules!" : "Review the modules and try again.";
            let emj = pct > 80 ? "🏆" : "📚";
            
            document.getElementById('r-msg').innerText = msg;
            document.getElementById('r-emoji').innerText = emj;
            showScreen('s-result');
        }}

        function saveToLeaderboard() {{
            let lb = JSON.parse(localStorage.getItem('cdac_lb') || '[]');
            lb.push({{ name: candidateName, score: score, total: db.length }});
            lb.sort((a, b) => b.score - a.score); // sort descending
            localStorage.setItem('cdac_lb', JSON.stringify(lb));
        }}

        function showLeaderboard() {{
            showScreen('s-lb');
            let lb = JSON.parse(localStorage.getItem('cdac_lb') || '[]');
            const list = document.getElementById('lb-list');
            list.innerHTML = '';
            
            if(lb.length === 0) {{
                list.innerHTML = '<div class="lbempty">No candidates have taken the exam yet.</div>';
                return;
            }}

            lb.forEach((entry, i) => {{
                let posColor = i === 0 ? 'g' : (i === 1 ? 's' : (i === 2 ? 'b' : ''));
                list.innerHTML += `
                    <div class="lbrow ${{entry.name === candidateName ? 'me' : ''}}">
                        <div class="lbpos ${{posColor}}">#${{i + 1}}</div>
                        <div class="lbinf">
                            <div class="lbnm">${{entry.name}}</div>
                        </div>
                        <div class="lbrt">
                            <div class="lbsc">${{entry.score}}/${{entry.total}}</div>
                        </div>
                    </div>
                `;
            }});
        }}
    </script>
</body>
</html>
'''

with open('/home/claude/final_exam.html', 'a') as f:
    f.write(html_content)

print(f"Part 2 written successfully. Exam generation complete. Open /home/claude/final_exam.html in your browser.")
PYEOF
