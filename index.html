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
@import url('https://fonts.googleapis.com/css2?family=Heebo:wght@300;400;700;900&display=swap');

/* עיצוב כללי */
html, body { font-family: 'Heebo', sans-serif; margin:0; padding:0; height:100%; background:#0d001a; overflow-x:hidden; }

/* אנימציות fade-in */
.fade-in { animation: fadeIn 0.6s ease-out forwards; opacity:0; }
@keyframes fadeIn { to { opacity:1; } }

/* Landing Page */
.landing-container { display:flex; flex-direction:column; align-items:center; justify-content:center; min-height:100vh; text-align:center; color:white; padding:20px; background: radial-gradient(circle at 20% 30%, #1a0033 0%, #0d001a 70%, #000 100%); transition:opacity 0.3s ease; }
.logo { font-size:5rem; font-weight:900; background: linear-gradient(135deg,#00ff88,#0066ff,#ff0099,#8a2be2); -webkit-background-clip:text; -webkit-text-fill-color:transparent; margin-bottom:20px; text-shadow:0 0 40px rgba(0,255,136,0.5); }
.landing-subtitle { font-size:1.5rem; color:rgba(255,255,255,0.8); margin-bottom:40px; text-shadow:0 0 20px rgba(0,255,136,0.3); }
.start-btn { padding:20px 60px; font-size:1.5rem; font-weight:bold; border-radius:50px; background:linear-gradient(135deg,#00ff88 0%,#0066ff 50%,#ff0099 100%); color:#000; cursor:pointer; box-shadow:0 20px 50px rgba(0,255,136,0.4),0 0 30px rgba(0,255,136,0.3); transition:all 0.3s ease; }
.start-btn:hover { transform:translateY(-5px) scale(1.05); box-shadow:0 30px 80px rgba(0,255,136,0.6),0 0 50px rgba(0,255,136,0.5); }

/* Editor & Preview */
.editor-container { min-height:500px; background:white; border-radius:16px; padding:24px; box-shadow:0 8px 16px rgba(0,0,0,0.2); overflow:auto; }
.preview-container { min-height:600px; background:white; border-radius:16px; padding:40px; box-shadow:0 8px 16px rgba(0,0,0,0.2); overflow:auto; }

/* Side panel */
.side-panel { background: rgba(255,255,255,0.95); border-radius:16px; padding:24px; box-shadow:0 6px 12px rgba(0,0,0,0.15); }

/* Buttons */
.nav-btn { cursor:pointer; border-radius:12px; padding:12px 24px; font-weight:600; display:flex; align-items:center; justify-content:center; gap:8px; transition:all 0.3s ease; }
.nav-btn:hover { transform: translateY(-2px); box-shadow:0 4px 12px rgba(0,0,0,0.2); }
</style>
</head>
<body>
<div id="root"></div>

<script type="text/babel">
const { useState, useEffect, useRef } = React;

/* מסך פתיחה */
const LandingPage = ({onStart}) => {
  return (
    <div className="landing-container fade-in">
      <div className="logo">עימוד AI</div>
      <div className="landing-subtitle">כלי מקצועי ומהיר לעימוד ספרים – חול וקודש</div>
      <button
        className="start-btn"
        onClick={() => {
          document.querySelector('.landing-container').style.opacity = 0;
          setTimeout(() => onStart(), 300);
        }}
      >להתחיל עכשיו</button>
    </div>
  );
};

/* אפליקציית עימוד */
const EditorApp = () => {
  const [settings,setSettings] = useState({columns:1,fontSize:16,lineHeight:1.8,margins:40,fontFamily:'Heebo',layout:'simple'});
  const [content,setContent] = useState('');
  const [bookTitle,setBookTitle] = useState('');
  const [authorName,setAuthorName] = useState('');
  const quillRef = useRef(null);
  const editorRef = useRef(null);

  useEffect(()=>{
    quillRef.current = new Quill('#editor',{theme:'snow',modules:{toolbar:[[{ 'header': [1,2,3,false] }],['bold','italic','underline','strike'],[{ 'align': [] }],[{ 'list':'ordered'},{'list':'bullet'}],[{ 'indent':'-1'},{'indent':'+1'}],['link','image'],['clean']]}});

    quillRef.current.on('text-change',()=>{
      setContent(quillRef.current.root.innerHTML);
      localStorage.setItem('book-layout-content',quillRef.current.root.innerHTML);
      updatePreview();
    });

    const savedContent = localStorage.getItem('book-layout-content');
    if(savedContent){ quillRef.current.root.innerHTML=savedContent; setContent(savedContent);}
    const savedSettings = localStorage.getItem('book-layout-settings');
    if(savedSettings){ setSettings(JSON.parse(savedSettings));}
  },[]);

  const handleSettingChange=(key,value)=>{
    setSettings(prev=>{
      const newSettings={...prev,[key]:value};
      localStorage.setItem('book-layout-settings',JSON.stringify(newSettings));
      updatePreview();
      return newSettings;
    });
  };

  const updatePreview=()=>{
    if(editorRef.current){
      editorRef.current.innerHTML=content;
      Object.assign(editorRef.current.style,{
        columnCount:settings.columns,
        fontSize:`${settings.fontSize}px`,
        lineHeight:settings.lineHeight,
        fontFamily:settings.fontFamily,
        padding:`${settings.margins}px`,
        textAlign:'justify',
        hyphens:'auto'
      });
    }
  };

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
    <div className="p-6 max-w-7xl mx-auto fade-in">
      <header className="text-center mb-8">
        <h1 className="text-5xl font-extrabold text-white mb-2"><i className="fas fa-columns mr-3"></i>עימוד AI</h1>
        <p className="text-xl text-indigo-200">כלי מקצועי ומהיר לעימוד ספרים – חול וקודש</p>
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

/* אפליקציה כוללת */
const App = () => {
  const [started,setStarted] = useState(false);
  return (
    <div className="fade-in">
      {started ? <EditorApp /> : <LandingPage onStart={()=>setStarted(true)} />}
    </div>
  );
};

ReactDOM.render(<App />, document.getElementById('root'));
</script>
</body>
</html>
