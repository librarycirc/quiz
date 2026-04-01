<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Library Assistant Master Quiz</title>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --navy:#003366;--navy-mid:#1a4f8a;--navy-light:#dbeafe;--navy-pale:#f0f6ff;
  --gold:#b8860b;--gold-light:#fef9c3;
  --green:#15803d;--green-light:#f0fdf4;--green-border:#86efac;
  --red:#b91c1c;--red-light:#fef2f2;--red-border:#fca5a5;
  --text:#1e293b;--muted:#64748b;--border:#e2e8f0;--bg:#f1f5f9;--surface:#fff;
  --r:10px;--rl:16px;
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;background:var(--bg);color:var(--text);min-height:100vh;padding:2rem 1rem 3rem}
.wrap{max-width:680px;margin:0 auto}

/* SCREENS */
.screen{display:none}.screen.active{display:block}

/* HEADER BAR */
.top-bar{background:var(--navy);color:#fff;border-radius:var(--rl);padding:1.4rem 1.75rem;margin-bottom:1.5rem;text-align:center}
.top-bar h1{font-size:20px;font-weight:700;letter-spacing:-.01em}
.top-bar p{font-size:13px;opacity:.75;margin-top:4px}

/* CARD */
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--rl);padding:1.75rem;margin-bottom:1.25rem}

/* START SCREEN */
.start-icon{font-size:48px;text-align:center;margin-bottom:1rem}
.start-title{font-size:22px;font-weight:700;color:var(--navy);text-align:center;margin-bottom:.5rem}
.start-sub{font-size:14px;color:var(--muted);text-align:center;margin-bottom:1.75rem;line-height:1.6}
.info-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:1.75rem}
.info-item{background:var(--navy-pale);border-radius:var(--r);padding:.9rem;text-align:center}
.info-val{font-size:22px;font-weight:700;color:var(--navy)}
.info-lbl{font-size:11px;color:var(--muted);margin-top:2px;text-transform:uppercase;letter-spacing:.05em}
.name-label{font-size:13px;font-weight:600;color:var(--navy);margin-bottom:6px;display:block;text-transform:uppercase;letter-spacing:.05em}
input[type=text]{width:100%;padding:10px 14px;font-size:15px;border:1.5px solid var(--border);border-radius:var(--r);font-family:inherit;color:var(--text);background:var(--bg);margin-bottom:14px;transition:border-color .15s}
input[type=text]:focus{outline:none;border-color:var(--navy)}

/* BUTTONS */
.btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;padding:11px 24px;font-size:14px;font-weight:600;border-radius:var(--r);border:none;cursor:pointer;font-family:inherit;transition:all .15s;text-align:center}
.btn-navy{background:var(--navy);color:#fff;width:100%}
.btn-navy:hover{background:var(--navy-mid)}
.btn-navy:disabled{background:#94a3b8;cursor:not-allowed}
.btn-outline{background:var(--surface);color:var(--navy);border:1.5px solid var(--navy)}
.btn-outline:hover{background:var(--navy-pale)}
.btn-green{background:var(--green);color:#fff}
.btn-green:hover{background:#166534}

/* QUIZ SCREEN */
.quiz-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:8px}
.q-counter{font-size:13px;font-weight:600;color:var(--navy);background:var(--navy-pale);padding:4px 12px;border-radius:20px}
.q-score{font-size:13px;font-weight:600;color:var(--muted)}
.progress-wrap{background:var(--border);border-radius:20px;height:6px;margin-bottom:1.5rem;overflow:hidden}
.progress-fill{height:6px;border-radius:20px;background:var(--navy);transition:width .4s}
.category-tag{display:inline-block;font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:.07em;padding:3px 10px;border-radius:20px;background:var(--navy-light);color:var(--navy);margin-bottom:12px}
.q-text{font-size:17px;font-weight:600;color:var(--text);line-height:1.5;margin-bottom:1.5rem}
.options{display:flex;flex-direction:column;gap:10px;margin-bottom:1.5rem}
.opt{display:flex;align-items:flex-start;gap:12px;padding:13px 16px;border:1.5px solid var(--border);border-radius:var(--r);cursor:pointer;transition:all .15s;background:var(--surface)}
.opt:hover:not(.disabled){border-color:var(--navy);background:var(--navy-pale)}
.opt.disabled{cursor:default}
.opt.correct{border-color:var(--green-border);background:var(--green-light)}
.opt.wrong{border-color:var(--red-border);background:var(--red-light)}
.opt.missed{border-color:var(--green-border);background:#f0fdf4;opacity:.75}
.opt-letter{width:26px;height:26px;border-radius:50%;background:var(--navy-pale);color:var(--navy);font-size:12px;font-weight:700;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:background .15s}
.opt.correct .opt-letter{background:var(--green);color:#fff}
.opt.wrong .opt-letter{background:var(--red);color:#fff}
.opt.missed .opt-letter{background:var(--green);color:#fff}
.opt-text{font-size:14px;color:var(--text);line-height:1.4;padding-top:2px}
.opt.correct .opt-text,.opt.missed .opt-text{color:var(--green);font-weight:500}
.opt.wrong .opt-text{color:var(--red);font-weight:500}
.feedback{border-radius:var(--r);padding:13px 16px;font-size:14px;line-height:1.6;display:none;margin-bottom:1.25rem}
.feedback.show{display:block}
.feedback.correct-fb{background:var(--green-light);border:1px solid var(--green-border);color:#14532d}
.feedback.wrong-fb{background:var(--red-light);border:1px solid var(--red-border);color:#7f1d1d}
.feedback strong{display:block;margin-bottom:4px;font-size:15px}

/* RESULT SCREEN */
.result-icon{font-size:56px;text-align:center;margin-bottom:.75rem}
.result-title{font-size:24px;font-weight:700;color:var(--navy);text-align:center;margin-bottom:.5rem}
.result-sub{font-size:14px;color:var(--muted);text-align:center;margin-bottom:1.5rem}
.score-ring{width:110px;height:110px;border-radius:50%;background:var(--navy);color:#fff;display:flex;flex-direction:column;align-items:center;justify-content:center;margin:0 auto 1.5rem}
.score-num{font-size:32px;font-weight:800}
.score-lbl{font-size:11px;opacity:.75;text-transform:uppercase;letter-spacing:.05em}
.result-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:1.5rem}
.rg-item{background:var(--navy-pale);border-radius:var(--r);padding:.9rem;text-align:center}
.rg-val{font-size:20px;font-weight:700;color:var(--navy)}
.rg-lbl{font-size:11px;color:var(--muted);margin-top:2px;text-transform:uppercase;letter-spacing:.04em}
.review-list{display:flex;flex-direction:column;gap:8px;margin-bottom:1.5rem}
.review-item{padding:10px 14px;border-radius:var(--r);font-size:13px;line-height:1.5}
.review-item.pass{background:var(--green-light);border:1px solid var(--green-border);color:#14532d}
.review-item.fail{background:var(--red-light);border:1px solid var(--red-border);color:#7f1d1d}
.review-q{font-weight:600;margin-bottom:2px}
.review-a{opacity:.85}
.result-actions{display:flex;gap:10px;flex-wrap:wrap}
.result-actions .btn{flex:1}

/* CERTIFICATE */
.cert-wrap{display:none;margin-top:1.5rem}
.cert-wrap.show{display:block}
.cert{background:var(--surface);border:3px solid var(--navy);border-radius:var(--rl);padding:2.5rem 2rem;text-align:center;position:relative;overflow:hidden}
.cert::before,.cert::after{content:'';position:absolute;width:120px;height:120px;border-radius:50%;background:var(--navy-pale);opacity:.5}
.cert::before{top:-40px;left:-40px}
.cert::after{bottom:-40px;right:-40px}
.cert-logo{font-size:36px;margin-bottom:.5rem}
.cert-institution{font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:.12em;color:var(--muted);margin-bottom:1rem}
.cert-headline{font-size:13px;color:var(--muted);margin-bottom:.35rem;text-transform:uppercase;letter-spacing:.08em}
.cert-name{font-size:30px;font-weight:800;color:var(--navy);margin-bottom:.75rem;font-style:italic}
.cert-body{font-size:13px;color:var(--muted);line-height:1.7;margin-bottom:1.25rem}
.cert-badge{display:inline-block;background:var(--navy);color:#fff;font-size:13px;font-weight:700;padding:7px 20px;border-radius:20px;letter-spacing:.04em;margin-bottom:1.25rem}
.cert-divider{border:none;border-top:1.5px solid var(--navy-light);margin:1rem 0}
.cert-footer{display:flex;justify-content:space-between;align-items:flex-end;flex-wrap:wrap;gap:12px}
.cert-sig{text-align:left}
.cert-sig-name{font-size:14px;font-weight:700;color:var(--navy)}
.cert-sig-role{font-size:11px;color:var(--muted)}
.cert-date{font-size:12px;color:var(--muted);text-align:right}
.cert-score{font-size:11px;color:var(--muted);background:var(--navy-pale);border-radius:var(--r);padding:4px 10px;margin-top:6px;display:inline-block}
.print-btn{margin-top:14px;width:100%}

@media print{
  body{background:#fff;padding:0}
  .wrap{max-width:100%}
  .screen:not(#screen-result){display:none!important}
  .result-actions,.print-btn,.review-list,.result-grid,.score-ring,.result-icon,.result-title,.result-sub{display:none!important}
  .cert-wrap{display:block!important}
  .cert{border:3px solid #003366;-webkit-print-color-adjust:exact;print-color-adjust:exact}
}
@media(max-width:480px){body{padding:1rem .75rem 2rem}.card{padding:1.25rem}.q-text{font-size:15px}}
</style>
</head>
<body>
<div class="wrap">

  <!-- TOP BAR -->
  <div class="top-bar">
    <h1>📚 Library Assistant Master Quiz</h1>
    <p>University Library — Student Worker Training</p>
  </div>

  <!-- START SCREEN -->
  <div class="screen active" id="screen-start">
    <div class="card">
      <div class="start-icon">🎓</div>
      <div class="start-title">Welcome to the Library Quiz</div>
      <div class="start-sub">Test your knowledge of library operations, patron service, and weekend procedures. Score 80% or above to earn your Library Assistant Certification!</div>
      <div class="info-grid">
        <div class="info-item"><div class="info-val">10</div><div class="info-lbl">Questions</div></div>
        <div class="info-item"><div class="info-val">80%</div><div class="info-lbl">Pass Score</div></div>
        <div class="info-item"><div class="info-val">🏅</div><div class="info-lbl">Certificate</div></div>
      </div>
      <label class="name-label" for="student-name">Your Full Name</label>
      <input type="text" id="student-name" placeholder="e.g. Jamie Lee" maxlength="60"/>
      <button class="btn btn-navy" id="start-btn" onclick="startQuiz()">Start Quiz →</button>
    </div>
  </div>

  <!-- QUIZ SCREEN -->
  <div class="screen" id="screen-quiz">
    <div class="card">
      <div class="quiz-header">
        <span class="q-counter" id="q-counter">Question 1 of 10</span>
        <span class="q-score" id="q-score">Score: 0</span>
      </div>
      <div class="progress-wrap"><div class="progress-fill" id="q-progress" style="width:0%"></div></div>
      <div class="category-tag" id="q-category"></div>
      <div class="q-text" id="q-text"></div>
      <div class="options" id="q-options"></div>
      <div class="feedback" id="q-feedback"></div>
      <button class="btn btn-navy" id="next-btn" style="display:none" onclick="nextQuestion()">Next Question →</button>
    </div>
  </div>

  <!-- RESULT SCREEN -->
  <div class="screen" id="screen-result">
    <div class="card">
      <div class="result-icon" id="res-icon"></div>
      <div class="result-title" id="res-title"></div>
      <div class="result-sub" id="res-sub"></div>
      <div class="score-ring"><div class="score-num" id="res-score-num"></div><div class="score-lbl">Score</div></div>
      <div class="result-grid">
        <div class="rg-item"><div class="rg-val" id="res-correct"></div><div class="rg-lbl">Correct</div></div>
        <div class="rg-item"><div class="rg-val" id="res-wrong"></div><div class="rg-lbl">Incorrect</div></div>
        <div class="rg-item"><div class="rg-val" id="res-pct"></div><div class="rg-lbl">Percentage</div></div>
      </div>
      <p style="font-size:13px;font-weight:600;color:var(--navy);margin-bottom:10px;text-transform:uppercase;letter-spacing:.05em;">Question Review</p>
      <div class="review-list" id="review-list"></div>
      <div class="result-actions">
        <button class="btn btn-outline" onclick="retakeQuiz()">↺ Retake Quiz</button>
        <button class="btn btn-green" id="cert-toggle-btn" style="display:none" onclick="toggleCert()">🏅 Show Certificate</button>
      </div>
    </div>

    <!-- CERTIFICATE -->
    <div class="cert-wrap" id="cert-wrap">
      <div class="cert">
        <div class="cert-logo">📚</div>
        <div class="cert-institution">University Library Services</div>
        <div class="cert-headline">This certifies that</div>
        <div class="cert-name" id="cert-name"></div>
        <div class="cert-body">has successfully completed the<br><strong>Library Student Assistant Training Quiz</strong><br>demonstrating proficiency in library operations, patron services,<br>shelving procedures, and weekend desk protocols.</div>
        <div class="cert-badge">✓ Library Assistant Certified</div>
        <div class="cert-score" id="cert-score-badge"></div>
        <div class="cert-divider"></div>
        <div class="cert-footer">
          <div class="cert-sig">
            <div class="cert-sig-name">Library Coordinator</div>
            <div class="cert-sig-role">University Library Services</div>
          </div>
          <div class="cert-date" id="cert-date"></div>
        </div>
      </div>
      <button class="btn btn-navy print-btn" onclick="window.print()">🖨 Print Certificate</button>
    </div>
  </div>

</div>
<script>
const QUESTIONS=[
  {
    cat:'Shelving',
    q:'When shelving a book, what is the FIRST thing you should check?',
    opts:['The title and author name','The location code (Circ, YA, Founders, etc.)','Whether the book is hardcover or paperback','The publication year'],
    ans:1,
    fb:'Correct! Always check the <strong>location code</strong> first (Circ, YA, Founders, etc.) to make sure you\'re shelving in the right section of the library before looking at the call number.'
  },
  {
    cat:'Call Numbers',
    q:'When two books share the same first line of a call number, what determines the correct shelving order?',
    opts:['The publication date','The book\'s color or size','The Cutter number (usually based on the author\'s name)','Alphabetical order by title'],
    ans:2,
    fb:'Correct! The <strong>Cutter number</strong> — usually derived from the author\'s last name — is used to distinguish books within the same classification and determine their order on the shelf.'
  },
  {
    cat:'Patron Service',
    q:'A patron approaches the desk looking confused and lost. What should you say first?',
    opts:['"Are you looking for something specific?"','"How can I help you today?"','"The catalog is on that computer over there."','"Do you have your library card?"'],
    ans:1,
    fb:'Correct! <strong>"How can I help you today?"</strong> is the ideal opener — it\'s warm, open-ended, and invites the patron to describe their needs in their own words without making assumptions.'
  },
  {
    cat:'Circulation',
    q:'A patron returns a book to the desk. What is the very FIRST step you should take?',
    opts:['Place it on the returns cart','Check it back in (scan/check-in) in the system','Ask the patron if they enjoyed the book','Check for any damage'],
    ans:1,
    fb:'Correct! Always <strong>check the item back in</strong> (scan it) in the circulation system first. This updates the system immediately, removes any holds, and prevents the patron from being charged for a late return.'
  },
  {
    cat:'Interlibrary Loan',
    q:'When receiving a returned Interlibrary Loan (ILL) book, what is the key difference from a regular return?',
    opts:['ILL books go directly back on the shelf','ILL books require a special check-in process — do NOT process them like regular returns','ILL books are always overdue','ILL books need a new barcode'],
    ans:1,
    fb:'Correct! ILL items must follow a <strong>special check-in procedure</strong>. Processing them as regular returns can cause errors. Always set them aside and follow the ILL-specific workflow or ask your coordinator.'
  },
  {
    cat:'Patron Service',
    q:'A patron asks you a question you don\'t know the answer to. What is the best response?',
    opts:['Make your best guess to avoid awkward silence','Apologize and say the library doesn\'t have that information','Be honest that you\'re unsure, then ask a supervisor or look it up and follow up','Tell them to check the library website'],
    ans:2,
    fb:'Correct! The professional approach is to <strong>be honest and take action</strong>. Saying "I\'m not sure, but let me find out for you" builds trust and ensures the patron gets accurate information.'
  },
  {
    cat:'Damage & Incidents',
    q:'You receive a returned book and notice it has significant water damage. What should you do?',
    opts:['Shelve it — it\'s not your problem','Throw it away to keep the collection clean','Document the damage and immediately notify your coordinator','Charge the patron on the spot'],
    ans:2,
    fb:'Correct! Always <strong>document the damage and report it to your coordinator</strong>. Never shelve a damaged item or take unilateral action. The coordinator will assess whether to charge the patron or withdraw the item.'
  },
  {
    cat:'Weekend Policy',
    q:'A community member (non-university affiliate) arrives at the library on Saturday morning. What do you tell them?',
    opts:['Let them in — they look trustworthy','Community members may only visit Monday–Friday, 9 AM – 5 PM; access is not permitted on weekends','Ask them to show a government ID and let them browse for 30 minutes','Offer them a guest pass'],
    ans:1,
    fb:'Correct! Community access is <strong>strictly Mon–Fri, 9 AM – 5 PM only</strong>. Politely but firmly explain the policy. No exceptions on weekends, regardless of circumstances.'
  },
  {
    cat:'Tech Support',
    q:'The book scanner stops working mid-shift. What is the correct 3-step troubleshooting sequence?',
    opts:['Call IT immediately → wait → give up','Exit and reopen the software → close all windows → restart the computer','Restart the computer right away → skip troubleshooting → post an "Out of Order" sign','Unplug the scanner → replug → shake it'],
    ans:1,
    fb:'Correct! Follow this order: (1) <strong>Exit and reopen</strong> the scanning application, (2) <strong>Close all open windows</strong> to clear memory, (3) <strong>Restart the computer</strong> fully. If none of these work, contact Campus Security or your supervisor.'
  },
  {
    cat:'End of Shift',
    q:'Before leaving at the end of your weekend shift, what is the required reporting procedure?',
    opts:['Send a text message to a colleague','Write a note on paper and leave it on the desk','Generate the end-of-day report in the dashboard, copy it, and paste it into the MS Teams library channel','Log out of the computer — no report needed on weekends'],
    ans:2,
    fb:'Correct! Always use the <strong>dashboard\'s "Generate & Copy" button</strong> to compile your shift summary (checklist status, scanner issues, incident reports, and notes), then paste it into the MS Teams library channel so the coordinator can review it Monday morning.'
  }
];

let currentQ=0,score=0,answers=[],studentName='';

function startQuiz(){
  const n=document.getElementById('student-name').value.trim();
  if(!n){document.getElementById('student-name').focus();document.getElementById('student-name').style.borderColor='#b91c1c';return;}
  document.getElementById('student-name').style.borderColor='';
  studentName=n;
  currentQ=0;score=0;answers=[];
  showScreen('screen-quiz');
  renderQuestion();
}

function renderQuestion(){
  const q=QUESTIONS[currentQ];
  document.getElementById('q-counter').textContent='Question '+(currentQ+1)+' of '+QUESTIONS.length;
  document.getElementById('q-score').textContent='Score: '+score;
  document.getElementById('q-progress').style.width=(currentQ/QUESTIONS.length*100)+'%';
  document.getElementById('q-category').textContent=q.cat;
  document.getElementById('q-text').textContent=q.q;
  const opts=document.getElementById('q-options');
  opts.innerHTML='';
  const letters=['A','B','C','D'];
  q.opts.forEach((o,i)=>{
    const div=document.createElement('div');
    div.className='opt';
    div.innerHTML='<div class="opt-letter">'+letters[i]+'</div><div class="opt-text">'+o+'</div>';
    div.onclick=()=>selectAnswer(i,div);
    opts.appendChild(div);
  });
  const fb=document.getElementById('q-feedback');
  fb.className='feedback';fb.innerHTML='';
  document.getElementById('next-btn').style.display='none';
}

function selectAnswer(idx,el){
  const q=QUESTIONS[currentQ];
  const allOpts=document.querySelectorAll('.opt');
  allOpts.forEach(o=>o.classList.add('disabled'));
  allOpts.forEach(o=>o.onclick=null);
  const correct=idx===q.ans;
  if(correct){score++;el.classList.add('correct');}
  else{
    el.classList.add('wrong');
    allOpts[q.ans].classList.add('missed');
  }
  answers.push({q:q.q,correct,yourAns:q.opts[idx],rightAns:q.opts[q.ans]});
  const fb=document.getElementById('q-feedback');
  if(correct){
    fb.className='feedback correct-fb show';
    fb.innerHTML='<strong>✅ Correct!</strong>'+q.fb;
  } else {
    fb.className='feedback wrong-fb show';
    fb.innerHTML='<strong>❌ Not quite.</strong> The correct answer is: <strong>'+q.opts[q.ans]+'</strong><br><br>'+q.fb;
  }
  const nextBtn=document.getElementById('next-btn');
  nextBtn.style.display='flex';
  nextBtn.textContent=currentQ===QUESTIONS.length-1?'See Results →':'Next Question →';
}

function nextQuestion(){
  currentQ++;
  if(currentQ>=QUESTIONS.length){showResults();}
  else{renderQuestion();}
}

function showResults(){
  showScreen('screen-result');
  const pct=Math.round(score/QUESTIONS.length*100);
  const pass=pct>=80;
  document.getElementById('res-icon').textContent=pass?'🏅':'📖';
  document.getElementById('res-title').textContent=pass?'Congratulations, '+studentName+'!':'Keep Studying, '+studentName+'!';
  document.getElementById('res-sub').textContent=pass
    ?'You passed with '+pct+'%! You\'ve earned your Library Assistant Certification.'
    :'You scored '+pct+'%. You need 80% to pass. Review the answers below and try again!';
  document.getElementById('res-score-num').textContent=pct+'%';
  document.getElementById('res-correct').textContent=score;
  document.getElementById('res-wrong').textContent=QUESTIONS.length-score;
  document.getElementById('res-pct').textContent=pct+'%';
  const reviewList=document.getElementById('review-list');
  reviewList.innerHTML='';
  answers.forEach((a,i)=>{
    const div=document.createElement('div');
    div.className='review-item '+(a.correct?'pass':'fail');
    div.innerHTML='<div class="review-q">'+(i+1)+'. '+a.q+'</div>'
      +'<div class="review-a">'+(a.correct?'✅ Your answer: '+a.yourAns:'❌ Your answer: '+a.yourAns+'<br>✅ Correct: '+a.rightAns)+'</div>';
    reviewList.appendChild(div);
  });
  if(pass){
    const certBtn=document.getElementById('cert-toggle-btn');
    certBtn.style.display='flex';
    const now=new Date();
    const months=['January','February','March','April','May','June','July','August','September','October','November','December'];
    document.getElementById('cert-name').textContent=studentName;
    document.getElementById('cert-date').textContent='Issued: '+months[now.getMonth()]+' '+now.getDate()+', '+now.getFullYear();
    document.getElementById('cert-score-badge').textContent='Final Score: '+pct+'% ('+score+'/'+QUESTIONS.length+' correct)';
  }
}

function toggleCert(){
  const wrap=document.getElementById('cert-wrap');
  const btn=document.getElementById('cert-toggle-btn');
  const show=!wrap.classList.contains('show');
  wrap.classList.toggle('show',show);
  btn.textContent=show?'🔼 Hide Certificate':'🏅 Show Certificate';
  if(show)wrap.scrollIntoView({behavior:'smooth',block:'start'});
}

function retakeQuiz(){
  currentQ=0;score=0;answers=[];
  document.getElementById('cert-wrap').classList.remove('show');
  const certBtn=document.getElementById('cert-toggle-btn');
  certBtn.style.display='none';
  certBtn.textContent='🏅 Show Certificate';
  showScreen('screen-quiz');
  renderQuestion();
}

function showScreen(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo({top:0,behavior:'smooth'});
}
</script>
</body>
</html>
