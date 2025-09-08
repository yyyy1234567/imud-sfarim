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
        .ql-container {
            direction: rtl;
            font-family: 'Heebo', sans-serif;
        }
        .ql-editor {
            min-height: 500px;
            background: white;
            border-radius: 12px;
            padding: 24px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
        }
        .fade-in {
            animation: fadeIn 0.4s ease-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .dark-mode {
            background: #1a202c;
            color: #f7fafc;
        }
        .dark-mode .ql-container {
            background: #2d3748;
            color: #f7fafc;
        }
        .dark-mode .bg-white {
            background: #2d3748 !important;
        }
        .dark-mode .text-gray-900 {
            color: #f7fafc !important;
        }
        .stepper-line {
            height: 2px;
            background: linear-gradient(to right, #4f46e5, #6366f1);
        }
        .stepper-circle {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            background: white;
            border: 2px solid #4f46e5;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #4f46e5;
            transition: all 0.3s ease;
        }
        .stepper-circle.active {
            background: #4f46e5;
            color: white;
        }
        .stepper-label {
            font-size: 0.875rem;
            color: #4f46e5;
            font-weight: 500;
            transition: color 0.3s ease;
        }
        .stepper-item.active .stepper-label {
            color: #4f46e5;
        }
    </style>
</head>
<body class="font-heebo bg-gradient-to-br from-indigo-500 via-indigo-600 to-indigo-700 min-h-screen transition-colors duration-300">
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useRef } = React;

        const App = () => {
            const [settings, setSettings] = useState({
                columns: 1,
                fontSize: 16,
                lineHeight: 1.8,
                margins: 40,
                fontFamily: 'Heebo',
            });
            const [activeTab, setActiveTab] = useState('editor');
            const [isWelcomeOpen, setIsWelcomeOpen] = useState(true);
            const [content, setContent] = useState('');
            const [bookTitle, setBookTitle] = useState('');
            const [authorName, setAuthorName] = useState('');
            const [isDarkMode, setIsDarkMode] = useState(false);
            const [stats, setStats] = useState({ words: 0, chars: 0, pages: 1 });
            const [currentStep, setCurrentStep] = useState(0);
            const quillRef = useRef(null);
            const editorRef = useRef(null);

            const workflowSteps = [
                { label: 'הגדרת דף', icon: 'columns', content: 'pageSetup' },
                { label: 'הגדרת סגנון', icon: 'palette', content: 'styleSetup' },
                { label: 'עריכה', icon: 'edit', content: 'editor' },
                { label: 'תצוגה מקדימה', icon: 'eye', content: 'preview' },
                { label: 'ייצוא', icon: 'download', content: 'export' },
            ];

            useEffect(() => {
                // Initialize Quill editor
                quillRef.current = new Quill('#editor', {
                    theme: 'snow',
                    modules: {
                        toolbar: [
                            [{ 'header': [1, 2, 3, 4, false] }],
                            ['bold', 'italic', 'underline', 'strike'],
                            [{ 'align': [] }],
                            [{ 'list': 'ordered' }, { 'list': 'bullet' }],
                            [{ 'indent': '-1' }, { 'indent': '+1' }],
                            ['link', 'image'],
                            ['clean']
                        ]
                    }
                });

                quillRef.current.on('text-change', () => {
                    setContent(quillRef.current.root.innerHTML);
                    autoSave();
                    updateStats();
                });

                // Load saved content
                const savedContent = localStorage.getItem('book-layout-content');
                if (savedContent) {
                    quillRef.current.root.innerHTML = savedContent;
                    setContent(savedContent);
                }

                const savedSettings = localStorage.getItem('book-layout-settings');
                if (savedSettings) {
                    setSettings(JSON.parse(savedSettings));
                }

                // Simulate real-time collaboration
                const interval = setInterval(() => {
                    console.log('Syncing content with server...');
                }, 30000);

                return () => clearInterval(interval);
            }, []);

            const autoSave = () => {
                localStorage.setItem('book-layout-content', quillRef.current.root.innerHTML);
                localStorage.setItem('book-layout-settings', JSON.stringify(settings));
            };

            const updateStats = () => {
                const text = quillRef.current.getText();
                const words = text.trim() === '' ? 0 : text.trim().split(/\s+/).length;
                const chars = text.length;
                const pages = Math.ceil(words / 250);
                setStats({ words, chars, pages });
            };

            const updatePreview = () => {
                if (editorRef.current) {
                    editorRef.current.innerHTML = content;
                    Object.assign(editorRef.current.style, {
                        columnCount: settings.columns,
                        fontSize: `${settings.fontSize}px`,
                        lineHeight: settings.lineHeight,
                        fontFamily: settings.fontFamily,
                        padding: `${settings.margins}px`,
                        textAlign: 'justify',
                        hyphens: 'auto',
                    });
                }
            };

            const switchTab = (tab) => {
                setActiveTab(tab);
                if (tab === 'preview') updatePreview();
                if (tab === 'export') updateStats();
            };

            const handleSettingChange = (key, value) => {
                setSettings(prev => {
                    const newSettings = { ...prev, [key]: value };
                    localStorage.setItem('book-layout-settings', JSON.stringify(newSettings));
                    if (key !== 'columns') {
                        quillRef.current.root.style[key] = key === 'fontSize' ? `${value}px` : key === 'lineHeight' ? value : value;
                    }
                    updatePreview();
                    return newSettings;
                });
            };

            const loadTemplate = (type) => {
                let template = '';
                switch (type) {
                    case 'novel':
                        template = `
                            <div style="text-align: center; margin-bottom: 60px; padding: 20px; border-bottom: 1px solid #e5e7eb;">
                                <h1 style="font-size: 36px; font-weight: bold; color: #4f46e5;">שם הרומן</h1>
                                <h2 style="font-size: 22px; margin-top: 16px; color: #6b7280;">מאת: ${authorName || '[שם המחבר]'}</h2>
                            </div>
                            <h2 style="font-size: 28px; font-weight: bold; margin: 40px 0 24px 0; color: #4f46e5;">פרק 1 - התחלה</h2>
                            <p>התחל לכתוב את הפרק הראשון של הרומן כאן... השתמש בכלי העיצוב כדי להוסיף אלמנטים מתקדמים.</p>
                        `;
                        break;
                    // Add more templates as needed
                    default:
                        template = '<p>התחל לכתוב כאן... השתמש בכלי העריכה המתקדמים.</p>';
                }
                quillRef.current.root.innerHTML = template;
                setContent(template);
                autoSave();
                switchTab('editor');
            };

            const exportPDF = () => {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF({
                    orientation: 'p',
                    unit: 'mm',
                    format: 'a4',
                    putOnlyUsedFonts: true,
                });
                doc.setFont(settings.fontFamily);
                doc.setFontSize(settings.fontSize);
                doc.html(quillRef.current.root, {
                    callback: () => {
                        doc.save(`${bookTitle || 'ספר'}.pdf`);
                    },
                    margin: [settings.margins / 2.834, settings.margins / 2.834, settings.margins / 2.834, settings.margins / 2.834],
                    autoPaging: 'text',
                    html2canvas: { scale: 0.3 },
                });
            };

            const exportWord = () => {
                const { Document, Packer, Paragraph, TextRun, AlignmentType } = window.docx;
                const doc = new Document({
                    sections: [{
                        properties: {
                            page: {
                                margin: { top: settings.margins * 28.35, right: settings.margins * 28.35, bottom: settings.margins * 28.35, left: settings.margins * 28.35 }
                            }
                        },
                        children: [
                            new Paragraph({
                                children: [new TextRun({ text: bookTitle, size: 48, bold: true })],
                                alignment: AlignmentType.CENTER,
                            }),
                            new Paragraph({
                                children: [new TextRun({ text: `מאת: ${authorName}`, size: 32 })],
                                alignment: AlignmentType.CENTER,
                                spacing: { after: 400 },
                            }),
                            new Paragraph({
                                children: [new TextRun({ text: quillRef.current.getText(), size: settings.fontSize * 2, font: settings.fontFamily })],
                                spacing: { line: settings.lineHeight * 240 },
                                alignment: AlignmentType.JUSTIFIED,
                            }),
                        ],
                    }],
                });
                Packer.toBlob(doc).then(blob => {
                    const url = URL.createObjectURL(blob);
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = `${bookTitle || 'ספר'}.docx`;
                    a.click();
                    URL.revokeObjectURL(url);
                });
            };

            const nextStep = () => {
                if (currentStep < workflowSteps.length - 1) {
                    setCurrentStep(currentStep + 1);
                    setActiveTab(workflowSteps[currentStep + 1].content);
                }
            };

            const prevStep = () => {
                if (currentStep > 0) {
                    setCurrentStep(currentStep - 1);
                    setActiveTab(workflowSteps[currentStep - 1].content);
                }
            };

            return (
                <div className={isDarkMode ? 'dark-mode' : ''}>
                    {isWelcomeOpen && (
                        <div className="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50 transition-opacity duration-500">
                            <div className="bg-white dark:bg-gray-900 p-10 rounded-3xl shadow-2xl max-w-lg mx-6 text-center transform scale-95 transition-transform duration-300">
                                <h2 className="text-3xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">
                                    <i className="fas fa-book-open mr-3"></i> ברוכים הבאים לעימוד AI!
                                </h2>
                                <p className="text-gray-700 dark:text-gray-300 mb-8 text-lg leading-relaxed">מערכת מקצועית ומתקדמת לעימוד ועיצוב ספרים בעברית, עם זרימת עבודה מודרנית וממשק אינטואיטיבי.</p>
                                <ul className="text-right mb-8 space-y-3">
                                    <li className="flex items-center justify-end"><span className="mr-3">עורך טקסט מתקדם עם AI</span><i className="fas fa-check text-green-500"></i></li>
                                    <li className="flex items-center justify-end"><span className="mr-3">תמיכה מלאה בעברית RTL</span><i className="fas fa-check text-green-500"></i></li>
                                    <li className="flex items-center justify-end"><span className="mr-3">זרימת עבודה מונחית</span><i className="fas fa-check text-green-500"></i></li>
                                    <li className="flex items-center justify-end"><span className="mr-3">ייצוא מקצועי ל-PDF/Word</span><i className="fas fa-check text-green-500"></i></li>
                                </ul>
                                <button
                                    onClick={() => setIsWelcomeOpen(false)}
                                    className="bg-indigo-600 text-white px-6 py-3 rounded-xl hover:bg-indigo-700 transition-all duration-300 hover:shadow-lg hover:-translate-y-1 flex items-center justify-center mx-auto"
                                >
                                    <i className="fas fa-rocket mr-3"></i> התחל עכשיו
                                </button>
                            </div>
                        </div>
                    )}

                    <div className="max-w-7xl mx-auto p-6">
                        <header className="bg-white dark:bg-gray-900 shadow-2xl rounded-2xl mb-10 overflow-hidden">
                            <div className="p-8 bg-gradient-to-r from-indigo-600 to-indigo-800 text-white text-center">
                                <h1 className="text-4xl font-extrabold flex items-center justify-center gap-4">
                                    <i className="fas fa-columns text-indigo-300"></i> עימוד AI
                                </h1>
                                <p className="text-xl opacity-90 mt-2">מערכת מקצועית לעיצוב ועימוד ספרים בעברית - מודרנית ויעילה</p>
                            </div>
                            <nav className="flex bg-gray-50 dark:bg-gray-800 px-6 py-4">
                                {workflowSteps.map((step, index) => (
                                    <button
                                        key={step.label}
                                        className={`flex-1 p-4 text-center ${activeTab === step.content ? 'bg-white dark:bg-gray-900 text-indigo-600 font-semibold' : 'text-gray-600 dark:text-gray-300'} hover:bg-gray-100 dark:hover:bg-gray-700 transition-all duration-300 rounded-lg mx-1`}
                                        onClick={() => {
                                            setCurrentStep(index);
                                            switchTab(step.content);
                                        }}
                                    >
                                        <i className={`fas fa-${step.icon} mr-2`}></i>
                                        {step.label}
                                    </button>
                                ))}
                            </nav>
                        </header>

                        {/* Workflow Stepper */}
                        <div className="mb-10">
                            <div className="flex justify-between items-center relative">
                                {workflowSteps.map((step, index) => (
                                    <div key={index} className={`stepper-item flex flex-col items-center flex-1 ${index <= currentStep ? 'active' : ''}`}>
                                        <div className="stepper-circle" style={{ background: index <= currentStep ? '#4f46e5' : 'white', color: index <= currentStep ? 'white' : '#4f46e5' }}>
                                            {index + 1}
                                        </div>
                                        <div className="stepper-label mt-2">{step.label}</div>
                                    </div>
                                ))}
                                <div className="absolute top-4 left-0 right-0 h-0.5 bg-gray-200 dark:bg-gray-700 z-[-1]">
                                    <div className="stepper-line" style={{ width: `${(currentStep / (workflowSteps.length - 1)) * 100}%` }}></div>
                                </div>
                            </div>
                            <div className="flex justify-between mt-6">
                                <button 
                                    onClick={prevStep}
                                    disabled={currentStep === 0}
                                    className="bg-gray-300 text-gray-700 px-6 py-3 rounded-xl disabled:opacity-50 hover:bg-gray-400 transition-all duration-300"
                                >
                                    <i className="fas fa-arrow-right mr-2"></i> הקודם
                                </button>
                                <button 
                                    onClick={nextStep}
                                    disabled={currentStep === workflowSteps.length - 1}
                                    className="bg-indigo-600 text-white px-6 py-3 rounded-xl disabled:opacity-50 hover:bg-indigo-700 transition-all duration-300 hover:shadow-md"
                                >
                                    הבא <i className="fas fa-arrow-left ml-2"></i>
                                </button>
                            </div>
                        </div>

                        <main className="grid grid-cols-1 lg:grid-cols-[350px_1fr] gap-10 bg-white dark:bg-gray-900 rounded-2xl shadow-2xl p-8 min-h-[700px] transition-all duration-300">
                            <aside className="bg-gray-50 dark:bg-gray-800 p-8 rounded-2xl border border-gray-200 dark:border-gray-700 space-y-8">
                                <div>
                                    <h3 className="text-xl font-semibold mb-6 flex items-center text-indigo-600 dark:text-indigo-400">
                                        <i className="fas fa-info-circle mr-3"></i> פרטי הספר
                                    </h3>
                                    <div className="space-y-4">
                                        <div>
                                            <label className="block font-medium mb-2 text-gray-700 dark:text-gray-300">כותרת הספר</label>
                                            <input
                                                type="text"
                                                className="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 dark:focus:ring-indigo-800 outline-none dark:bg-gray-700 transition-all duration-300"
                                                value={bookTitle}
                                                onChange={(e) => setBookTitle(e.target.value)}
                                                placeholder="הזן כותרת לספר"
                                            />
                                        </div>
                                        <div>
                                            <label className="block font-medium mb-2 text-gray-700 dark:text-gray-300">שם המחבר</label>
                                            <input
                                                type="text"
                                                className="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 dark:focus:ring-indigo-800 outline-none dark:bg-gray-700 transition-all duration-300"
                                                value={authorName}
                                                onChange={(e) => setAuthorName(e.target.value)}
                                                placeholder="הזן שם מחבר"
                                            />
                                        </div>
                                    </div>
                                </div>

                                <div>
                                    <h3 className="text-xl font-semibold mb-6 flex items-center text-indigo-600 dark:text-indigo-400">
                                        <i className="fas fa-tools mr-3"></i> פעולות
                                    </h3>
                                    <div className="grid grid-cols-1 gap-4">
                                        <button className="bg-green-500 text-white p-4 rounded-xl hover:bg-green-600 transition-all duration-300 hover:shadow-md flex items-center justify-center" onClick={() => loadTemplate('sample')}>
                                            <i className="fas fa-file-text mr-3"></i> הוסף טקסט דוגמה
                                        </button>
                                        <button className="bg-blue-500 text-white p-4 rounded-xl hover:bg-blue-600 transition-all duration-300 hover:shadow-md flex items-center justify-center" onClick={updateStats}>
                                            <i className="fas fa-calculator mr-3"></i> הצג סטטיסטיקות
                                        </button>
                                        <button className="bg-yellow-500 text-white p-4 rounded-xl hover:bg-yellow-600 transition-all duration-300 hover:shadow-md flex items-center justify-center" onClick={() => { if (window.confirm('בטוח שברצונך לנקות?')) quillRef.current.root.innerHTML = ''; }}>
                                            <i className="fas fa-trash mr-3"></i> נקה תוכן
                                        </button>
                                        <button className="bg-gray-500 text-white p-4 rounded-xl hover:bg-gray-600 transition-all duration-300 hover:shadow-md flex items-center justify-center" onClick={autoSave}>
                                            <i className="fas fa-save mr-3"></i> שמור שינויים
                                        </button>
                                        <button className="bg-purple-500 text-white p-4 rounded-xl hover:bg-purple-600 transition-all duration-300 hover:shadow-md flex items-center justify-center" onClick={() => setIsDarkMode(!isDarkMode)}>
                                            <i className="fas fa-moon mr-3"></i> {isDarkMode ? 'מצב בהיר' : 'מצב כהה'}
                                        </button>
                                    </div>
                                </div>

                                <div>
                                    <h3 className="text-xl font-semibold mb-6 flex items-center text-indigo-600 dark:text-indigo-400">
                                        <i className="fas fa-chart-bar mr-3"></i> סטטיסטיקות
                                    </h3>
                                    <div className="grid grid-cols-3 gap-4">
                                        <div className="bg-white dark:bg-gray-700 p-4 rounded-xl shadow text-center transition-all duration-300 hover:shadow-md">
                                            <span className="block text-3xl font-bold text-indigo-600">{stats.words}</span>
                                            <div className="text-gray-600 dark:text-gray-300 text-sm font-medium">מילים</div>
                                        </div>
                                        <div className="bg-white dark:bg-gray-700 p-4 rounded-xl shadow text-center transition-all duration-300 hover:shadow-md">
                                            <span className="block text-3xl font-bold text-indigo-600">{stats.chars}</span>
                                            <div className="text-gray-600 dark:text-gray-300 text-sm font-medium">תווים</div>
                                        </div>
                                        <div className="bg-white dark:bg-gray-700 p-4 rounded-xl shadow text-center transition-all duration-300 hover:shadow-md">
                                            <span className="block text-3xl font-bold text-indigo-600">{stats.pages}</span>
                                            <div className="text-gray-600 dark:text-gray-300 text-sm font-medium">עמודים</div>
                                        </div>
                                    </div>
                                </div>
                            </aside>

                            <section className="p-8 bg-gray-50 dark:bg-gray-800 rounded-2xl">
                                {activeTab === 'pageSetup' && (
                                    <div className="fade-in space-y-8">
                                        <h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">שלב 1: הגדרת דף</h2>
                                        <div className="space-y-6">
                                            <div>
                                                <label className="block font-medium mb-3 text-gray-700 dark:text-gray-300">מספר עמודות: {settings.columns}</label>
                                                <input
                                                    type="range"
                                                    className="w-full h-3 bg-gray-200 dark:bg-gray-600 rounded-full cursor-pointer appearance-none transition-all duration-300"
                                                    min="1"
                                                    max="4"
                                                    value={settings.columns}
                                                    onChange={(e) => handleSettingChange('columns', parseInt(e.target.value))}
                                                />
                                            </div>
                                            <div>
                                                <label className="block font-medium mb-3 text-gray-700 dark:text-gray-300">שוליים: {settings.margins}px</label>
                                                <input
                                                    type="range"
                                                    className="w-full h-3 bg-gray-200 dark:bg-gray-600 rounded-full cursor-pointer appearance-none transition-all duration-300"
                                                    min="20"
                                                    max="100"
                                                    step="5"
                                                    value={settings.margins}
                                                    onChange={(e) => handleSettingChange('margins', parseInt(e.target.value))}
                                                />
                                            </div>
                                        </div>
                                    </div>
                                )}
                                {activeTab === 'styleSetup' && (
                                    <div className="fade-in space-y-8">
                                        <h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">שלב 2: הגדרת סגנון</h2>
                                        <div className="space-y-6">
                                            <div>
                                                <label className="block font-medium mb-3 text-gray-700 dark:text-gray-300">גודל גופן: {settings.fontSize}px</label>
                                                <input
                                                    type="range"
                                                    className="w-full h-3 bg-gray-200 dark:bg-gray-600 rounded-full cursor-pointer appearance-none transition-all duration-300"
                                                    min="12"
                                                    max="32"
                                                    value={settings.fontSize}
                                                    onChange={(e) => handleSettingChange('fontSize', parseInt(e.target.value))}
                                                />
                                            </div>
                                            <div>
                                                <label className="block font-medium mb-3 text-gray-700 dark:text-gray-300">גובה שורה: {settings.lineHeight}</label>
                                                <input
                                                    type="range"
                                                    className="w-full h-3 bg-gray-200 dark:bg-gray-600 rounded-full cursor-pointer appearance-none transition-all duration-300"
                                                    min="1.2"
                                                    max="3.0"
                                                    step="0.1"
                                                    value={settings.lineHeight}
                                                    onChange={(e) => handleSettingChange('lineHeight', parseFloat(e.target.value))}
                                                />
                                            </div>
                                            <div>
                                                <label className="block font-medium mb-3 text-gray-700 dark:text-gray-300">סוג גופן</label>
                                                <select
                                                    className="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 dark:focus:ring-indigo-800 outline-none dark:bg-gray-700 transition-all duration-300"
                                                    value={settings.fontFamily}
                                                    onChange={(e) => handleSettingChange('fontFamily', e.target.value)}
                                                >
                                                    <option value="Heebo">Heebo (מודרני)</option>
                                                    <option value="Assistant">Assistant (קלאסי)</option>
                                                    <option value="David Libre">David Libre (מסורתי)</option>
                                                    <option value="Times New Roman">Times New Roman (סטנדרטי)</option>
                                                    <option value="Arial">Arial (פשוט)</option>
                                                </select>
                                            </div>
                                        </div>
                                    </div>
                                )}
                                {activeTab === 'editor' && (
                                    <div className="fade-in">
                                        <h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">שלב 3: עריכת תוכן</h2>
                                        <div id="editor" className="ql-container"></div>
                                    </div>
                                )}
                                {activeTab === 'preview' && (
                                    <div className="fade-in">
                                        <h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">שלב 4: תצוגה מקדימה</h2>
                                        <div ref={editorRef} className="bg-white dark:bg-gray-700 p-10 rounded-2xl shadow-xl min-h-[600px] overflow-auto transition-all duration-300"></div>
                                    </div>
                                )}
                                {activeTab === 'templates' && (
                                    <div className="fade-in">
                                        <h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">תבניות מוכנות</h2>
                                        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                                            {['novel', 'poetry', 'academic', 'children', 'textbook', 'magazine'].map(type => (
                                                <div
                                                    key={type}
                                                    className="bg-white dark:bg-gray-700 border border-gray-200 dark:border-gray-600 rounded-2xl overflow-hidden cursor-pointer hover:border-indigo-500 hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2"
                                                    onClick={() => loadTemplate(type)}
                                                >
                                                    <div className="h-40 bg-gradient-to-br from-indigo-100 to-indigo-200 dark:from-indigo-900 dark:to-indigo-800 flex items-center justify-center text-5xl text-indigo-500 dark:text-indigo-300">
                                                        <i className={`fas fa-${type === 'novel' ? 'book' : type === 'poetry' ? 'feather-alt' : type === 'academic' ? 'graduation-cap' : type === 'children' ? 'child' : type === 'textbook' ? 'book-open' : 'newspaper'}`}></i>
                                                    </div>
                                                    <div className="p-6">
                                                        <div className="font-bold text-lg text-gray-800 dark:text-gray-200 mb-2">{type === 'novel' ? 'רומן' : type === 'poetry' ? 'שירה' : type === 'academic' ? 'אקדמי' : type === 'children' ? 'ספר ילדים' : type === 'textbook' ? 'ספר לימוד' : 'מגזין'}</div>
                                                        <div className="text-gray-600 dark:text-gray-400 text-sm">
                                                            {type === 'novel' ? 'תבנית מקצועית לרומן עם פרקים' : type === 'poetry' ? 'תבנית לאסופת שירים פיוטית' : type === 'academic' ? 'תבנית למאמרים אקדמיים' : type === 'children' ? 'תבנית צבעונית לספרי ילדים' : type === 'textbook' ? 'תבנית מובנית לספרי לימוד' : 'תבנית דינמית למגזינים'}
                                                        </div>
                                                    </div>
                                                </div>
                                            ))}
                                        </div>
                                    </div>
                                )}
                                {activeTab === 'export' && (
                                    <div className="fade-in space-y-8">
                                        <h2 className="text-2xl font-bold text-indigo-600 dark:text-indigo-400 mb-6">שלב 5: ייצוא הספר</h2>
                                        <div className="grid grid-cols-1 sm:grid-cols-3 gap-6 mb-8">
                                            <div className="bg-white dark:bg-gray-700 p-6 rounded-2xl shadow text-center transition-all duration-300 hover:shadow-md">
                                                <span className="block text-3xl font-bold text-indigo-600">{stats.words}</span>
                                                <div className="text-gray-600 dark:text-gray-300 text-sm font-medium">מילים</div>
                                            </div>
                                            <div className="bg-white dark:bg-gray-700 p-6 rounded-2xl shadow text-center transition-all duration-300 hover:shadow-md">
                                                <span className="block text-3xl font-bold text-indigo-600">{stats.pages}</span>
                                                <div className="text-gray-600 dark:text-gray-300 text-sm font-medium">עמודים</div>
                                            </div>
                                            <div className="bg-white dark:bg-gray-700 p-6 rounded-2xl shadow text-center transition-all duration-300 hover:shadow-md">
                                                <span className="block text-3xl font-bold text-indigo-600">{stats.chars}</span>
                                                <div className="text-gray-600 dark:text-gray-300 text-sm font-medium">תווים</div>
                                            </div>
                                        </div>
                                        <div className="flex flex-wrap gap-6 justify-center">
                                            <button className="bg-indigo-600 text-white px-8 py-4 rounded-2xl hover:bg-indigo-700 transition-all duration-300 hover:shadow-lg flex items-center" onClick={exportPDF}>
                                                <i className="fas fa-file-pdf mr-3"></i> ייצוא ל-PDF
                                            </button>
                                            <button className="bg-blue-500 text-white px-8 py-4 rounded-2xl hover:bg-blue-600 transition-all duration-300 hover:shadow-lg flex items-center" onClick={exportWord}>
                                                <i className="fas fa-file-word mr-3"></i> ייצוא ל-Word
                                            </button>
                                            <button className="bg-green-500 text-white px-8 py-4 rounded-2xl hover:bg-green-600 transition-all duration-300 hover:shadow-lg flex items-center" onClick={() => {}}>
                                                <i className="fas fa-file-code mr-3"></i> ייצוא ל-HTML
                                            </button>
                                        </div>
                                    </div>
                                )}
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
