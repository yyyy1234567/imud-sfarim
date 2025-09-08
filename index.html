<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>עימוד AI - מערכת מקצועית</title>
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
body, .ql-container { direction: rtl; font-family:'Heebo', sans-serif; }
.ql-editor { min-height:500px; background:white; border-radius:16px; padding:24px; box-shadow:0 6px 12px rgba(0,0,0,0.1); line-height:1.8; }
.fade-in { animation: fadeIn 0.4s ease-out; }
@keyframes fadeIn { from {opacity:0; transform:translateY(20px);} to {opacity:1; transform:translateY(0);} }
.dark-mode { background:#1a202c; color:#f7fafc; }
.dark-mode .ql-container { background:#2d3748; color:#f7fafc; }
.dark-mode .bg-white { background:#2d3748 !important; }
.dark-mode .text-gray-900 { color:#f7fafc !important; }
.stepper-line { height:2px; background:linear-gradient(to right,#4f46e5,#6366f1); }
.stepper-circle { width:32px; height:32px; border-radius:50%; background:white; border:2px solid #4f46e5; display:flex; align-items:center; justify-content:center; font-weight:bold; color:#4f46e5; transition:all 0.3s ease; }
.stepper-circle.active { background:#4f46e5; color:white; }
.stepper-label { font-size:0.875rem; color:#4f46e5; font-weight:500; transition: color 0.3s ease; }
.gemara-layout { display:flex; gap:20px; }
.gemara-layout .rashi, .gemara-layout .tosefot { width:50%; padding:10px; box-shadow:0 2px 6px rgba(0,0,0,0.05); background:#fdfdfd; border-radius:12px; }
.gemara-layout .rashi { text-align:right; }
.gemara-layout .tosefot { text-align:left; }
</style>
</head>
<body class="font-heebo bg-gradient-to-br from-indigo-500 via-indigo-600 to-indigo-700 min-h-screen transition-colors duration-300">
<div id="root"></div>
<script type="text/babel">
const { useState, useEffect, useRef } = React;
const App = () => {
    const [settings,setSettings] = useState({columns:1,fontSize:16,lineHeight:1.8,margins:40,fontFamily:'Heebo'});
    const [activeTab,setActiveTab] = useState('editor');
    const [content,setContent] = useState('');
    const [bookTitle,setBookTitle] = useState('');
    const [authorName,setAuthorName] = useState('');
    const [isDarkMode,setIsDarkMode] = useState(false);
    const [stats,setStats] = useState({words:0,chars:0,pages:1});
    const [currentStep,setCurrentStep] = useState(0);
    const quillRef = useRef(null);
    const previewRef = useRef(null);
    const workflowSteps = [
        {label:'הגדרת דף',icon:'columns',content:'pageSetup'},
        {label:'הגדרת סגנון',icon:'palette',content:'styleSetup'},
        {label:'עריכה',icon:'edit',content:'editor'},
        {label:'תצוגה מקדימה',icon:'eye',content:'preview'},
        {label:'ייצוא',icon:'download',content:'export'}
    ];

    useEffect(()=>{
        quillRef.current = new Quill('#editor',{theme:'snow',modules:{toolbar:[['bold','italic','underline'],['link','image'],[{align:[]}],[{list:'ordered'},{list:'bullet'}],['clean']] }});
        quillRef.current.on('text-change',()=>{ setContent(quillRef.current.root.innerHTML); autoSave(); updateStats(); });
        const savedContent = localStorage.getItem('book-layout-content'); if(savedContent){ quillRef.current.root.innerHTML=savedContent; setContent(savedContent);}
        const savedSettings = localStorage.getItem('book-layout-settings'); if(savedSettings){ setSettings(JSON.parse(savedSettings)); }
    },[]);

    const autoSave = ()=>{ localStorage.setItem('book-layout-content',quillRef.current.root.innerHTML); localStorage.setItem('book-layout-settings',JSON.stringify(settings)); };
    const updateStats = ()=>{
        const text = quillRef.current.getText();
        const words = text.trim()===''?0:text.trim().split(/\s+/).length;
        const chars = text.length;
        const pages = Math.ceil(words/250);
        setStats({words,chars,pages});
    };
    const handleSettingChange=(key,value)=>{ setSettings(prev=>({...prev,[key]:value})); updatePreview(); };
    const switchTab=(tab)=>{ setActiveTab(tab); if(tab==='preview') updatePreview(); };
    const updatePreview=()=>{
        if(previewRef.current){ previewRef.current.innerHTML=content; previewRef.current.style.columnCount=settings.columns; previewRef.current.style.fontSize=settings.fontSize+'px'; previewRef.current.style.lineHeight=settings.lineHeight; previewRef.current.style.padding=settings.margins+'px'; previewRef.current.style.fontFamily=settings.fontFamily; }
    };

    const loadGemaraExample=()=>{
        const example = `
        <div class="gemara-layout">
            <div class="rashi"><p>רש"י: דברי רש"י על ההתחלה...</p><p>עוד רש"י...</p></div>
            <div class="tosefot"><p>תוספות: דברי תוספות בהרחבה...</p><p>עוד תוספות...</p></div>
        </div>`;
        quillRef.current.root.innerHTML=example; setContent(example); autoSave(); switchTab('editor');
    };

    const nextStep=()=>{ if(currentStep<workflowSteps.length-1){ setCurrentStep(currentStep+1); switchTab(workflowSteps[currentStep+1].content); } };
    const prevStep=()=>{ if(currentStep>0){ setCurrentStep(currentStep-1); switchTab(workflowSteps[currentStep-1].content); } };

    return(
        <div className={isDarkMode?'dark-mode':''}>
            <div className="max-w-7xl mx-auto p-6">
                <header className="bg-white dark:bg-gray-900 shadow-2xl rounded-2xl mb-10 p-6 text-center">
                    <h1 className="text-4xl font-extrabold text-indigo-600">עימוד AI - מערכת מקצועית</h1>
                    <p className="text-lg mt-2 text-gray-700 dark:text-gray-300">עורך מתקדם, עימוד רש"י ותוספות, רב-טקסט, יצוא PDF/Word</p>
                </header>
                {/* Stepper */}
                <div className="flex justify-between mb-6 relative">
                    {workflowSteps.map((step,index)=>(
                        <div key={index} className={`flex flex-col items-center flex-1 ${index<=currentStep?'active':''}`}>
                            <div className={`stepper-circle ${index<=currentStep?'active':''}`}>{index+1}</div>
                            <div className="stepper-label mt-2">{step.label}</div>
                        </div>
                    ))}
                    <div className="absolute top-4 left-0 right-0 h-0.5 bg-gray-200 dark:bg-gray-700 z-[-1]">
                        <div className="stepper-line" style={{width:`${(currentStep/(workflowSteps.length-1))*100}%`}}></div>
                    </div>
                </div>

                <div className="grid grid-cols-1 lg:grid-cols-[300px_1fr] gap-6">
                    <aside className="bg-gray-50 dark:bg-gray-800 p-6 rounded-xl space-y-4 shadow-lg">
                        <div>
                            <label className="font-semibold">כותרת הספר</label>
                            <input type="text" value={bookTitle} onChange={e=>setBookTitle(e.target.value)}
                                className="w-full p-3 mt-1 rounded-lg border border-gray-300 dark:border-gray-600 focus:ring-2 focus:ring-indigo-300"/>
                        </div>
                        <div>
                            <label className="font-semibold">שם המחבר</label>
                            <input type="text" value={authorName} onChange={e=>setAuthorName(e.target.value)}
                                className="w-full p-3 mt-1 rounded-lg border border-gray-300 dark:border-gray-600 focus:ring-2 focus:ring-indigo-300"/>
                        </div>
                        <button className="w-full bg-indigo-600 text-white p-3 rounded-xl mt-3 hover:bg-indigo-700 transition-all" onClick={loadGemaraExample}>טען עימוד רש"י ותוספות</button>
                        <button className="w-full bg-green-500 text-white p-3 rounded-xl mt-1 hover:bg-green-600 transition-all" onClick={()=>switchTab('preview')}>תצוגה מקדימה</button>
                        <button className="w-full bg-gray-500 text-white p-3 rounded-xl mt-1 hover:bg-gray-600 transition-all" onClick={()=>setIsDarkMode(!isDarkMode)}>{isDarkMode?'מצב בהיר':'מצב כהה'}</button>
                        <button className="w-full bg-indigo-400 text-white p-3 rounded-xl mt-1 hover:bg-indigo-500 transition-all" onClick={prevStep}><i className="fas fa-arrow-right ml-2"></i> הקודם</button>
                        <button className="w-full bg-indigo-800 text-white p-3 rounded-xl mt-1 hover:bg-indigo-900 transition-all" onClick={nextStep}>הבא <i className="fas fa-arrow-left mr-2"></i></button>
                    </aside>

                    <main className="bg-white dark:bg-gray-900 p-
