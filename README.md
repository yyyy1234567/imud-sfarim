<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>עימוד AI - מערכת מקצועית ונוצצת</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.22.5/babel.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/docx@8.5.0/build/index.js"></script>
<link href="https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.snow.css" rel="stylesheet">
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
<style>
body { font-family: 'Heebo', sans-serif; margin:0; overflow-x:hidden; background:#1e293b; }
.fade-in { animation: fadeIn 0.6s ease-out; }
@keyframes fadeIn { from {opacity:0; transform:translateY(20px);} to {opacity:1; transform:translateY(0);} }
.slide-up { animation: slideUp 0.8s ease-out; }
@keyframes slideUp { from {opacity:0; transform:translateY(50px);} to {opacity:1; transform:translateY(0);} }
.splash { position:fixed; inset:0; background:#4f46e5; display:flex; align-items:center; justify-content:center; color:white; font-size:2rem; font-weight:bold; flex-direction:column; z-index:9999; }
.splash-logo { font-size:3rem; animation:spin 2s linear infinite; margin-bottom:20px;}
@keyframes spin { from {transform:rotate(0deg);} to {transform:rotate(360deg);} }
.ql-container { direction: rtl; font-family: 'Heebo', sans-serif; background:white; border-radius:12px; padding:20px; min-height:500px; box-shadow:0 4px 12px rgba(0,0,0,0.1);}
.dark-mode { background:#111827; color:#f7fafc; }
.dark-mode .ql-container { background:#1f2937; color:#f7fafc; }
button { transition:all 0.3s ease; }
button:hover { transform:translateY(-2px); box-shadow:0 4px 12px rgba(0,0,0,0.2); }
</style>
</head>
<body>
<div id="splash" class="splash">
    <div class="splash-logo"><i class="fas fa-columns"></i></div>
    <div>טוען את מערכת העימוד...</div>
</div>
<div id="root" class="hidden"></div>
<script type="text/babel">
const { useState, useEffect, useRef } = React;

const App = () => {
    const [settings, setSettings] = useState({ columns:1, fontSize:16, lineHeight:1.8, margins:40, fontFamily:'Heebo', layout:'simple' });
    const [activeTab, setActiveTab] = useState('editor');
    const [content, setContent] = useState('');
    const [bookTitle, setBookTitle] = useState('');
    const [authorName, setAuthorName] = useState('');
    const [isDarkMode, setIsDarkMode] = useState(false);
    const [stats, setStats] = useState({ words:0, chars:0, pages:1 });
    const quillRef = useRef(null);
    const editorRef = useRef(null);

    const workflowSteps = [
        { label:'הגדרת דף', icon:'columns', content:'pageSetup' },
        { label:'הגדרת סגנון', icon:'palette', content:'styleSetup' },
        { label:'עריכה', icon:'edit', content:'editor' },
        { label:'תצוגה מקדימה', icon:'eye', content:'preview' },
        { label:'ייצוא', icon:'download', content:'export' }
    ];

    useEffect(()=>{
        // הסרה של מסך הפתיחה לאחר טעינה
        setTimeout(()=>{ document.getElementById('splash').style.display='none'; document.getElementById('root').classList.remove('hidden'); }, 1800);

        quillRef.current = new Quill('#editor', { theme:'snow', modules:{ toolbar:[
            [{ 'header':[1,2,3,4,false] }],
            ['bold','italic','underline','strike'],
            [{ 'align':[] }],
            [{ 'list':'ordered' },{ 'list':'bullet' }],
            [{ 'indent':'-1'},{ 'indent':'+1' }],
            ['link','image'],
            ['clean']
        ]}});
        quillRef.current.on('text-change', ()=>{
            setContent(quillRef.current.root.innerHTML);
            autoSave();
            updateStats();
        });
        const saved = localStorage.getItem('book-layout-content');
        if(saved) { quillRef.current.root.innerHTML = saved; setContent(saved);}
        const savedSettings = localStorage.getItem('book-layout-settings');
        if(savedSettings) setSettings(JSON.parse(savedSettings));
    },[]);

    const autoSave = ()=>{ localStorage.setItem('book-layout-content', quillRef.current.root.innerHTML); localStorage.setItem('book-layout-settings', JSON.stringify(settings)); }
    const updateStats = ()=>{ const text = quillRef.current.getText(); const words = text.trim()===''?0:text.trim().split(/\s+/).length; const chars=text.length; const pages=Math.ceil(words/250); setStats({ words, chars, pages }); }
    const updatePreview = ()=>{
        if(editorRef.current){
            editorRef.current.innerHTML = '';
            if(settings.layout==='simple'){
                editorRef.current.innerHTML = content;
            } else {
                const parser = new DOMParser();
                const doc = parser.parseFromString(content,'text/html');
                const lines = doc.body.innerHTML.split(/\n|<p>/).filter(l=>l.trim()!=='');
                const container = document.createElement('div');
                container.style.display='grid';
                container.style.gridTemplateColumns='1fr 1fr';
                container.style.columnGap='40px';
                lines.forEach(line=>{
                    const left = document.createElement('div');
                    left.innerHTML = line;
                    const right = document.createElement('div');
                    right.innerHTML = line;
                    container.appendChild(left);
                    container.appendChild(right);
                });
                editorRef.current.appendChild(container);
            }
            Object.assign(editorRef.current.style,{
                columnCount: settings.columns,
                fontSize: settings.fontSize+'px',
                lineHeight: settings.lineHeight,
                fontFamily: settings.fontFamily,
                padding: settings.margins+'px',
                textAlign:'justify',
                hyphens:'auto'
            });
        }
    };

    const switchTab=(tab)=>{ setActiveTab(tab); if(tab==='preview') updatePreview(); if(tab==='export') updateStats(); }
    const handleSettingChange=(key,value)=>{ setSettings(prev=>{ const newS={...prev,[key]:value}; localStorage.setItem('book-layout-settings',JSON.stringify(newS)); updatePreview(); return newS;}); }

    const exportPDF=()=>{
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({ orientation:'p', unit:'mm', format:'a4', putOnlyUsedFonts:true });
        doc.setFont(settings.fontFamily);
        doc.setFontSize(settings.fontSize);
        doc.html(quillRef.current.root, { callback:()=>{ doc.save((bookTitle||'ספר')+'.pdf'); }, margin:[settings.margins/2.834,settings.margins/2.834,settings.margins/2.834,settings.margins/2.834], autoPaging:'text', html2canvas:{scale:0.3} });
    };

    const exportWord=()=>{
        const { Document,Packer,Paragraph,TextRun,AlignmentType } = window.docx;
        const doc = new Document({ sections:[{ properties:{ page:{ margin:{ top:settings.margins*28.35,right:settings.margins*28.35,bottom:settings.margins*28.35,left:settings.margins*28.35 }}}, children:[
            new Paragraph({ children:[new TextRun({ text:bookTitle,size:48,bold:true })], alignment:AlignmentType.CENTER }),
            new Paragraph({ children:[new TextRun({ text:`מאת: ${authorName}`, size:32 })], alignment:AlignmentType.CENTER, spacing:{after:400} }),
            new Paragraph({ children:[new TextRun({ text:quillRef.current.getText(), size:settings.fontSize*2, font:settings.fontFamily })], spacing:{line:settings.lineHeight*240}, alignment:AlignmentType.JUSTIFIED })
        ]}]});
        Packer.toBlob(doc).then(blob=>{ const url=URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download=(bookTitle||'ספר')+'.docx'; a.click(); URL.revokeObjectURL(url); });
    };

    return(
        <div className={isDarkMode?'dark-mode fade-in':'fade-in'}>
            <div className="max-w-7xl mx-auto p-6">
                <header className="bg-gradient-to-r from-indigo-600 to-indigo-800 shadow-2xl rounded-2xl mb-10 overflow-hidden p-8 text-white text-center">
                    <h1 className="text-4xl font-extrabold flex items-center justify-center gap-4 slide-up"><i className="fas fa-columns text-indigo-200"></i> עימוד AI</h1>
                    <p className="text-xl opacity-90 mt-2">מערכת מקצועית לעיצוב ועימוד ספרים בעברית - מודרנית ויעילה</p>
                </header>
                <nav className="flex bg-gray-100 dark:bg-gray-800 px-6 py-4 mb-6 rounded-xl">
                    {workflowSteps.map((step,index)=>(
                        <button key={step.label} className={`flex-1 p-4 text-center ${activeTab===step.content?'bg-white dark:bg-gray-900 text-indigo-600 font-semibold':'text-gray-600 dark:text-gray-300'} hover:bg-gray-200 dark:hover:bg-gray-700 rounded-lg mx-1`} onClick={()=>switchTab(step.content)}>
                            <i className={`fas fa-${step.icon} mr-2`}></i> {step.label}
                        </button>
                    ))}
                </nav>
                <main className="grid grid-cols-1 lg:grid-cols-[350px_1fr] gap-10 bg-white dark:bg-gray-900 rounded-2xl shadow-2xl p-8 min-h-[700px] transition-all duration-500">
                    <aside className="bg-gray-50 dark:bg-gray-800 p-8 rounded-2xl border border-gray-200 dark:border-gray-700 space-y-8 fade-in">
                        <div>
                            <h3 className="text-xl font-semibold mb-6 text-indigo-600 dark:text-indigo-400"><i className="fas fa-info-circle mr-3"></i> פרטי הספר</h3>
                            <input type="text" placeholder="כותרת הספר" className="w-full mb-4 p-3 rounded-xl border" value={bookTitle} onChange={(e)=>setBookTitle(e.target.value)} />
                            <input type="text" placeholder="שם המחבר" className="w-full mb-4 p-3 rounded-xl border" value={authorName} onChange={(e)=>setAuthorName(e.target.value)} />
                        </div>
                        <div className="grid grid-cols-1 gap-4">
                            <button className="bg-green-500 text-white p-4 rounded-xl" onClick={()=>quillRef.current.root.innerHTML='<p>טקסט לדוגמה</p>'}>הוסף טקסט דוגמה</button>
                            <button className="bg-blue-500 text-white p-4 rounded-xl" onClick={updateStats}>הצג סטטיסטיקות</button>
                            <button className="bg-yellow-500 text-white p-4 rounded-xl" onClick={()=>{if(window.confirm('בטוח לנקות?')) quillRef.current.root.innerHTML=''; updateStats();}}>נקה תוכן</button>
                            <button className="bg-gray-500 text-white p-4 rounded-xl" onClick={autoSave}>שמור שינויים</button>
                            <button className="bg-purple-500 text-white p-4 rounded-xl" onClick={()=>setIsDarkMode(!isDarkMode)}>{isDarkMode?'מצב בהיר':'מצב כהה'}</button>
                        </div>
                    </aside>

                    <section className="p-8 bg-gray-50 dark:bg-gray-800 rounded-2xl fade-in">
                        {activeTab==='editor' && <div><h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6 slide-up">עריכת תוכן</h2><div id="editor" className="ql-container"></div></div>}
                        {activeTab==='preview' && <div><h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6 slide-up">תצוגה מקדימה</h2><div ref={editorRef} className="bg-white dark:bg-gray-700 p-10 rounded-2xl shadow-xl min-h-[600px] overflow-auto transition-all duration-300"></div></div>}
                        {activeTab==='export' && <div className="space-y-8"><h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6 slide-up">ייצוא</h2>
                        <div className="flex gap-6">
                            <button className="bg-indigo-600 text-white px-8 py-4 rounded-2xl hover:bg-indigo-700" onClick={exportPDF}>ייצוא PDF</button>
                            <button className="bg-blue-500 text-white px-8 py-4 rounded-2xl hover:bg-blue-600" onClick={exportWord}>ייצוא Word</button>
                        </div>
                        </div>}
                    </section>
                </main>
            </div>
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById('root'));
</script>
</body>
</html>
