<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>עימוד AI - מערכת מקצועית לעימוד ספרים</title>
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
html, body { font-family: 'Heebo', sans-serif; background: linear-gradient(to bottom right,#4f46e5,#6366f1); min-height: 100vh; margin:0; padding:0; }
.fade-in { animation: fadeIn 0.4s ease-out; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
button { transition: all 0.3s ease; }
.editor-container { min-height: 500px; background: white; border-radius: 16px; padding: 24px; box-shadow: 0 8px 16px rgba(0,0,0,0.2); overflow-auto; }
.preview-container { min-height: 600px; background: white; border-radius: 16px; padding: 40px; box-shadow: 0 8px 16px rgba(0,0,0,0.2); overflow-auto; }
.side-panel { background: rgba(255,255,255,0.95); border-radius:16px; padding:24px; box-shadow:0 6px 12px rgba(0,0,0,0.15); }
.nav-btn { cursor:pointer; border-radius:12px; padding:12px 24px; font-weight:600; display:flex; align-items:center; justify-content:center; gap:8px; }
.nav-btn:hover { transform: translateY(-2px); box-shadow:0 4px 12px rgba(0,0,0,0.2); }
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useEffect, useRef } = React;

const App = () => {
const [settings,setSettings] = useState({columns:1,fontSize:16,lineHeight:1.8,margins:40,fontFamily:'Heebo',layout:'simple'});
const [activeTab,setActiveTab] = useState('editor');
const [content,setContent] = useState('');
const [bookTitle,setBookTitle] = useState('');
const [authorName,setAuthorName] = useState('');
const quillRef = useRef(null);
const editorRef = useRef(null);
const workflowSteps = [
{label:'הגדרת דף',icon:'columns',content:'pageSetup'},
{label:'הגדרת סגנון',icon:'palette',content:'styleSetup'},
{label:'עריכה',icon:'edit',content:'editor'},
{label:'תצוגה מקדימה',icon:'eye',content:'preview'},
{label:'ייצוא',icon:'download',content:'export'},
];

useEffect(()=>{
quillRef.current = new Quill('#editor',{theme:'snow',modules:{toolbar:[[{ 'header': [1,2,3,false] }],['bold','italic','underline','strike'],[{ 'align': [] }],[{ 'list':'ordered'},{'list':'bullet'}],[{ 'indent':'-1'},{'indent':'+1'}],['link','image'],['clean']]}});

quillRef.current.on('text-change',()=>{
setContent(quillRef.current.root.innerHTML);
autoSave();
updatePreview();
});

const savedContent = localStorage.getItem('book-layout-content');
if(savedContent){ quillRef.current.root.innerHTML=savedContent; setContent(savedContent);}
const savedSettings = localStorage.getItem('book-layout-settings');
if(savedSettings){ setSettings(JSON.parse(savedSettings));}
},[]);

const autoSave=()=>{localStorage.setItem('book-layout-content',quillRef.current.root.innerHTML);localStorage.setItem('book-layout-settings',JSON.stringify(settings));};
const handleSettingChange=(key,value)=>{setSettings(prev=>{const newSettings={...prev,[key]:value};localStorage.setItem('book-layout-settings',JSON.stringify(newSettings));updatePreview();return newSettings;});};
const updatePreview=()=>{if(editorRef.current){editorRef.current.innerHTML=content;Object.assign(editorRef.current.style,{columnCount:settings.columns,fontSize:`${settings.fontSize}px`,lineHeight:settings.lineHeight,fontFamily:settings.fontFamily,padding:`${settings.margins}px`,textAlign:'justify',hyphens:'auto'});}};

const exportPDF=()=>{
const { jsPDF } = window.jspdf;
const doc=new jsPDF({orientation:'p',unit:'mm',format:'a4',putOnlyUsedFonts:true});
doc.setFont(settings.fontFamily);
doc.setFontSize(settings.fontSize);
doc.html(quillRef.current.root,{callback:()=>{doc.save(`${bookTitle||'ספר'}.pdf`);},margin:[settings.margins/2.834,settings.margins/2.834,settings.margins/2.834,settings.margins/2.834],autoPaging:'text',html2canvas:{scale:0.3},});
};

const exportWord=()=>{
const { Document,Packer,Paragraph,TextRun,AlignmentType } = window.docx;
const doc=new Document({sections:[{properties:{page:{margin:{top:settings.margins*28.35,right:settings.margins*28.35,bottom:settings.margins*28.35,left:settings.margins*28.35}}},children:[
new Paragraph({children:[new TextRun({text:bookTitle,size:48,bold:true})],alignment:AlignmentType.CENTER}),
new Paragraph({children:[new TextRun({text:`מאת: ${authorName}`,size:32})],alignment:AlignmentType.CENTER,spacing:{after:400}}),
new Paragraph({children:[new TextRun({text:quillRef.current.getText(),size:settings.fontSize*2,font:settings.fontFamily})],spacing:{line:settings.lineHeight*240},alignment:AlignmentType.JUSTIFIED}),
]}]});
Packer.toBlob(doc).then(blob=>{const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download=`${bookTitle||'ספר'}.docx`;a.click();URL.revokeObjectURL(url);});
};

return (
<div className="p-6 max-w-7xl mx-auto">
<header className="text-center mb-8">
<h1 className="text-5xl font-extrabold text-white mb-2"><i className="fas fa-columns mr-3"></i>עימוד AI</h1>
<p className="text-xl text-indigo-200">מערכת מודרנית ונוצצת לעימוד ספרים בעברית</p>
</header>

<div className="grid grid-cols-1 lg:grid-cols-[300px_1fr] gap-6">
<aside className="side-panel space-y-6">
<h2 className="text-2xl font-bold text-indigo-600 mb-4">מסלול עימוד</h2>
<div className="flex flex-col gap-4">
<button className="nav-btn bg-indigo-500 text-white" onClick={()=>handleSettingChange('layout','simple')}>עימוד פשוט</button>
<button className="nav-btn bg-purple-500 text-white" onClick={()=>handleSettingChange('layout','multi')}>עימוד רב-טקסט</button>
</div>

<h2 className="text-2xl font-bold text-indigo-600 mt-8 mb-4">הגדרות עימוד</h2>
<div className="space-y-4">
<label>גודל גופן: {settings.fontSize}px</label>
<input type="range" min="12" max="32" value={settings.fontSize} onChange={(e)=>handleSettingChange('fontSize',parseInt(e.target.value))} className="w-full"/>
<label>גובה שורה: {settings.lineHeight}</label>
<input type="range" min="1.2" max="3.0" step="0.1" value={settings.lineHeight} onChange={(e)=>handleSettingChange('lineHeight',parseFloat(e.target.value))} className="w-full"/>
<label>שוליים: {settings.margins}px</label>
<input type="range" min="20" max="100" step="5" value={settings.margins} onChange={(e)=>handleSettingChange('margins',parseInt(e.target.value))} className="w-full"/>
<label>סוג גופן</label>
<select value={settings.fontFamily} onChange={(e)=>handleSettingChange('fontFamily',e.target.value)} className="w-full p-2 border rounded">
<option value="Heebo">Heebo (מודרני)</option>
<option value="Assistant">Assistant (קלאסי)</option>
<option value="David Libre">David Libre (מסורתי)</option>
<option value="Times New Roman">Times New Roman</option>
<option value="Arial">Arial</option>
</select>
</div>

<h2 className="text-2xl font-bold text-indigo-600 mt-8 mb-4">פרטי הספר</h2>
<input type="text" placeholder="כותרת הספר" value={bookTitle} onChange={(e)=>setBookTitle(e.target.value)} className="w-full p-2 border rounded"/>
<input type="text" placeholder="שם המחבר" value={authorName} onChange={(e)=>setAuthorName(e.target.value)} className="w-full p-2 border rounded mt-2"/>

<h2 className="text-2xl font-bold text-indigo-600 mt-8 mb-4">ייצוא</h2>
<div className="flex flex-col gap-3">
<button className="nav-btn bg-red-500 text-white" onClick={exportPDF}><i className="fas fa-file-pdf mr-2"></i>PDF</button>
<button className="nav-btn bg-blue-500 text-white" onClick={exportWord}><i className="fas fa-file-word mr-2"></i>Word</button>
</div>
</aside>

<main className="flex flex-col gap-6">
<div className="editor-container" id="editor"></div>
<h2 className="text-2xl font-bold text-indigo-600 mt-6 mb-4">תצוגה מקדימה</h2>
<div ref={editorRef} className="preview-container"></div>
</main>
</div>
</div>
);
};

ReactDOM.render(<App />, document.getElementById('root'));
</script>
</body>
</html>
