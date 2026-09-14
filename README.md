# futurify-ai
import React, { useState, useEffect } from 'react';
import { 
  Globe, Shield, Brain, BarChart3, Zap, BookOpen, Download, 
  CheckCircle, ArrowRight, Star, Lock, Mail, ChevronRight, 
  Filter, Search, ShoppingBag, Eye, RefreshCw, FileText, 
  Sliders, User, Settings, AlertCircle, Sparkles, Check, DollarSign
} from 'lucide-react';

// ==========================================
// MOCK DATA: REPORTS & DIGITAL PRODUCTS
// ==========================================
const INITIAL_REPORTS = [
  {
    id: 'rep-1',
    title: {
      ar: 'تقرير WEF 2026: مستقبل الوظائف وتأثير الذكاء الاصطناعي التوليدي',
      en: 'WEF Report 2026: The Future of Jobs & Generative AI Impact'
    },
    category: 'AI & Automation',
    date: '2026-08-15',
    readTime: '5 min',
    source: 'World Economic Forum',
    summary: {
      ar: 'تحليل دقيق لخارطة التحول الوظيفي العالمية حتى عام 2030، والمهارات الـ10 الأكثر طلباً في عصر الأتمتة المتقدمة.',
      en: 'In-depth analysis of the global job transformation roadmap through 2030, highlighting top 10 in-demand skills in the automation era.'
    },
    keyTakeaways: [
      { ar: 'إعادة تأهيل 44% من المهارات الجوهرية للقوى العاملة.', en: '44% of core workforce skills will require reskilling.' },
      { ar: 'نمو الوظائف المعتمدة على الهندسة الفورية (Prompt Engineering) بنسبة 65%.', en: '65% growth in roles requiring prompt engineering.' }
    ],
    verified: true
  },
  {
    id: 'rep-2',
    title: {
      ar: 'دراسة Gartner: معايير الدفاع السيبراني للشركات الناشئة والمؤسسات',
      en: 'Gartner Study: Cybersecurity Defense Standards for Enterprises'
    },
    category: 'Cybersecurity',
    date: '2026-07-22',
    readTime: '7 min',
    source: 'Gartner Research',
    summary: {
      ar: 'دليل استراتيجي لمواجهة التهديدات المعتمدة على الذكاء الاصطناعي وهندسة حماية البيانات الحساسة.',
      en: 'Strategic guide to countering AI-driven cyber threats and architectural safeguarding of sensitive data.'
    },
    keyTakeaways: [
      { ar: 'اعتماد نموذج Zero Trust يقلل الاختراقات بنسبة 70%.', en: 'Zero Trust adoption reduces breaches by up to 70%.' },
      { ar: 'أهمية أتمتة الاستجابة للحوادث السيبرانية.', en: 'Critical necessity of automated cyber incident response.' }
    ],
    verified: true
  }
];

const INITIAL_PRODUCTS = [
  {
    id: 'prod-1',
    title: {
      ar: 'الدليل الشامل للهندسة الفورية وأتمتة بيئة العمل بالذكاء الاصطناعي',
      en: 'The Ultimate Guide to Prompt Engineering & AI Workplace Automation'
    },
    category: 'AI & Automation',
    priceUSD: 29.99,
    priceMAD: 290,
    rating: 4.9,
    salesCount: 1420,
    format: 'PDF + Worksheets',
    description: {
      ar: 'دليل عملي يحتوي على 150+ نموذج حافز (Prompts) جاهز للتطبيق المباشر لتضاعف إنتاجيتك في بيئة العمل.',
      en: 'A practical guide with 150+ battle-tested prompt templates to 10x your daily workplace productivity.'
    },
    badge: 'Best Seller'
  },
  {
    id: 'prod-2',
    title: {
      ar: 'قائمة المراجعة التنفيذية للامتثال والأمن السيبراني للمؤسسات',
      en: 'Executive Cybersecurity & Compliance Checklist for Organizations'
    },
    category: 'Cybersecurity',
    priceUSD: 19.99,
    priceMAD: 190,
    rating: 4.8,
    salesCount: 890,
    format: 'Interactive Audit Sheet',
    description: {
      ar: 'أداة تدقيق شاملة لتحديد الفجوات الأمنية وحماية البيانات وفق المعايير الدولية المعتمدة.',
      en: 'Comprehensive audit tool to identify security gaps and ensure data compliance with international standards.'
    },
    badge: 'Essential'
  },
  {
    id: 'prod-3',
    title: {
      ar: 'حقيبة تحليل البيانات الضخمة وبناء لوحات القيادة الاستراتيجية',
      en: 'Big Data Analytics & Strategic Dashboard Construction Toolkit'
    },
    category: 'Data Analytics',
    priceUSD: 39.99,
    priceMAD: 390,
    rating: 5.0,
    salesCount: 650,
    format: 'Templates + Datasets',
    description: {
      ar: 'قوالب جاهزة وتحليلات متقدمة لتنفيذ القرارات المعتمدة على البيانات الضخمة وسرعة اتخاذ القرار.',
      en: 'Ready-to-use templates and advanced analytics frameworks for data-driven decision making.'
    },
    badge: 'Popular'
  },
  {
    id: 'prod-4',
    title: {
      ar: 'دليل مهارات التحول الرقمي والإنتاجية الشخصية 2026',
      en: 'Digital Transformation & Personal Productivity Handbook 2026'
    },
    category: 'Productivity',
    priceUSD: 14.99,
    priceMAD: 140,
    rating: 4.7,
    salesCount: 2100,
    format: 'E-Book (PDF/EPUB)',
    description: {
      ar: 'خطوات موجهة للموظفين والمحترفين لبناء مسار مهني غير قابل للاستغناء عنه في عصر الذكاء الاصطناعي.',
      en: 'Actionable steps for professionals to build an indispensable career path in the AI-driven era.'
    },
    badge: 'Featured'
  }
];

export default function App() {
  // State Management
  const [lang, setLang] = useState('ar'); // 'ar' | 'en'
  const [activeTab, setActiveTab] = useState('home'); // 'home' | 'reports' | 'store' | 'academy' | 'admin' | 'contact'
  const [currency, setCurrency] = useState('USD'); // 'USD' | 'MAD'
  const [products, setProducts] = useState(INITIAL_PRODUCTS);
  const [reports, setReports] = useState(INITIAL_REPORTS);
  const [selectedCategory, setSelectedCategory] = useState('All');
  const [cart, setCart] = useState([]);
  const [notification, setNotification] = useState('');

  // Admin Dashboard States
  const [newReportTitleAr, setNewReportTitleAr] = useState('');
  const [newReportTitleEn, setNewReportTitleEn] = useState('');
  const [newReportCategory, setNewReportCategory] = useState('AI & Automation');
  const [newReportSummaryAr, setNewReportSummaryAr] = useState('');
  const [newReportSummaryEn, setNewReportSummaryEn] = useState('');

  // Direction Sync
  useEffect(() => {
    document.dir = lang === 'ar' ? 'rtl' : 'ltr';
  }, [lang]);

  const showToast = (msg) => {
    setNotification(msg);
    setTimeout(() => setNotification(''), 3500);
  };

  const addToCart = (product) => {
    setCart([...cart, product]);
    showToast(lang === 'ar' ? 'تمت إضافة المنتج إلى السلة بنجاح' : 'Product added to cart successfully');
  };

  const handleCreateReport = (e) => {
    e.preventDefault();
    if (!newReportTitleAr || !newReportTitleEn) return;

    const created = {
      id: `rep-${Date.now()}`,
      title: { ar: newReportTitleAr, en: newReportTitleEn },
      category: newReportCategory,
      date: new Date().toISOString().split('T')[0],
      readTime: '4 min',
      source: 'Futurify AI Intelligence',
      summary: { ar: newReportSummaryAr, en: newReportSummaryEn },
      keyTakeaways: [
        { ar: 'تم الفحص والتدقيق التلقائي بواسطة الذكاء الاصطناعي.', en: 'Automated auditing & verification completed via AI.' }
      ],
      verified: true
    };

    setReports([created, ...reports]);
    setNewReportTitleAr('');
    setNewReportTitleEn('');
    setNewReportSummaryAr('');
    setNewReportSummaryEn('');
    showToast(lang === 'ar' ? 'تم اعتماد ونشر التقرير بنجاح!' : 'Report verified & published successfully!');
  };

  // Translations Object
  const t = {
    brand: 'Futurify AI',
    tagline: lang === 'ar' ? 'منصة التمكين الرقمي ومعرفة المستقبل' : 'Digital Empowerment & Future Intelligence Platform',
    nav: {
      home: lang === 'ar' ? 'الرئيسة' : 'Home',
      reports: lang === 'ar' ? 'التقارير والدراسات' : 'Global Reports',
      store: lang === 'ar' ? 'متجر الأدلة الرقمية' : 'Digital Storefront',
      academy: lang === 'ar' ? 'الأكاديمية' : 'Skills Academy',
      admin: lang === 'ar' ? 'لوحة التدقيق والتحكم' : 'Curation Dashboard',
      contact: lang === 'ar' ? 'التواصل والدعم' : 'Contact & Network'
    },
    hero: {
      badge: lang === 'ar' ? 'محرك المعرفة المستقبلية الموثوقة 2026' : 'Trusted Future Intelligence Hub 2026',
      title: lang === 'ar' ? 'استشراف المستقبل... وتطوير المهارات الرقمية بدقة عالية' : 'Anticipating the Future... Precision Digital Skill Empowerment',
      subtitle: lang === 'ar' ? 'منصة متخصصة في تنقية وتلخيص التقارير الدولية، وإنتاج الأدلة والمنتجات الرقمية القابلة لتوليد الدخل والمصممة خصيصاً لمستقبل العمل مع الذكاء الاصطناعي.' : 'A specialized platform curating international reports, generating income-ready digital toolkits designed for the future of work with AI.',
      ctaStore: lang === 'ar' ? 'استكشف المنتجات الرقمية' : 'Browse Digital Toolkits',
      ctaReports: lang === 'ar' ? 'قراءة أحدث التحاليل' : 'Read Latest Intelligence'
    },
    categories: {
      All: lang === 'ar' ? 'الكل' : 'All Domains',
      'AI & Automation': lang === 'ar' ? 'الذكاء الاصطناعي والأتمتة' : 'AI & Automation',
      Cybersecurity: lang === 'ar' ? 'الأمن السيبراني' : 'Cybersecurity',
      'Data Analytics': lang === 'ar' ? 'تحليل البيانات الضخمة' : 'Data Analytics',
      Productivity: lang === 'ar' ? 'الإنتاجية المتقدمة' : 'Productivity Tools'
    }
  };

  const filteredProducts = selectedCategory === 'All' 
    ? products 
    : products.filter(p => p.category === selectedCategory);

  const filteredReports = selectedCategory === 'All' 
    ? reports 
    : reports.filter(r => r.category === selectedCategory);

  return (
    <div className={`min-h-screen bg-slate-950 text-slate-100 font-sans selection:bg-cyan-500 selection:text-slate-950 ${lang === 'ar' ? 'text-right' : 'text-left'}`}>
      
      {/* TOAST NOTIFICATION */}
      {notification && (
        <div className="fixed top-5 left-1/2 -translate-x-1/2 z-50 bg-cyan-500 text-slate-950 px-6 py-3 rounded-full font-bold shadow-lg shadow-cyan-500/30 flex items-center gap-2 transition-all">
          <Sparkles className="w-5 h-5" />
          <span>{notification}</span>
        </div>
      )}

      {/* HEADER / NAVIGATION */}
      <header className="sticky top-0 z-40 backdrop-blur-md bg-slate-950/80 border-b border-slate-800/80">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
          
          {/* Logo & Brand */}
          <div className="flex items-center gap-3 cursor-pointer" onClick={() => setActiveTab('home')}>
            <div className="w-10 h-10 rounded-xl bg-gradient-to-tr from-cyan-500 via-blue-600 to-indigo-600 flex items-center justify-center shadow-lg shadow-cyan-500/20">
              <Brain className="w-6 h-6 text-white" />
            </div>
            <div>
              <span className="text-xl font-extrabold tracking-tight bg-gradient-to-r from-white via-slate-200 to-slate-400 bg-clip-text text-transparent">
                {t.brand}
              </span>
              <span className="block text-[10px] text-cyan-400 font-semibold uppercase tracking-wider">
                Intelligence
              </span>
            </div>
          </div>

          {/* Desktop Navigation Links */}
          <nav className="hidden md:flex items-center gap-1 bg-slate-900/60 p-1.5 rounded-2xl border border-slate-800">
            {Object.keys(t.nav).map((key) => (
              <button
                key={key}
                onClick={() => setActiveTab(key)}
                className={`px-4 py-2 rounded-xl text-sm font-medium transition-all ${
                  activeTab === key
                    ? 'bg-gradient-to-r from-cyan-500 to-blue-600 text-white shadow-md shadow-cyan-500/20'
                    : 'text-slate-400 hover:text-white hover:bg-slate-800/50'
                }`}
              >
                {t.nav[key]}
              </button>
            ))}
          </nav>

          {/* Header Controls: Currency & Language Toggle */}
          <div className="flex items-center gap-3">
            <button
              onClick={() => setCurrency(currency === 'USD' ? 'MAD' : 'USD')}
              className="hidden sm:flex items-center gap-1.5 px-3 py-1.5 rounded-lg border border-slate-800 bg-slate-900 text-xs text-slate-300 hover:border-slate-700 transition"
            >
              <DollarSign className="w-3.5 h-3.5 text-cyan-400" />
              <span>{currency}</span>
            </button>

            <button
              onClick={() => setLang(lang === 'ar' ? 'en' : 'ar')}
              className="flex items-center gap-2 px-3.5 py-1.5 rounded-xl border border-cyan-500/30 bg-cyan-500/10 text-cyan-400 text-xs font-semibold hover:bg-cyan-500/20 transition"
            >
              <Globe className="w-4 h-4" />
              <span>{lang === 'ar' ? 'English' : 'العربية'}</span>
            </button>
          </div>
        </div>
      </header>

      {/* MOBILE NAVIGATION BAR */}
      <div className="md:hidden flex overflow-x-auto gap-2 p-3 bg-slate-900 border-b border-slate-800 scrollbar-none">
        {Object.keys(t.nav).map((key) => (
          <button
            key={key}
            onClick={() => setActiveTab(key)}
            className={`px-3.5 py-1.5 rounded-lg text-xs whitespace-nowrap font-medium ${
              activeTab === key ? 'bg-cyan-500 text-slate-950 font-bold' : 'bg-slate-800 text-slate-300'
            }`}
          >
            {t.nav[key]}
          </button>
        ))}
      </div>

      {/* MAIN CONTENT AREA */}
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">

        {/* ========================================== */}
        {/* TAB 1: HOME PAGE HERO & HIGHLIGHTS         */}
        {/* ========================================== */}
        {activeTab === 'home' && (
          <div className="space-y-16">
            
            {/* HERO SECTION */}
            <div className="relative rounded-3xl bg-gradient-to-b from-slate-900 via-slate-900/90 to-slate-950 p-8 sm:p-14 border border-slate-800 overflow-hidden">
              <div className="absolute -top-24 -left-24 w-96 h-96 bg-cyan-500/10 rounded-full blur-3xl pointer-events-none" />
              <div className="absolute -bottom-24 -right-24 w-96 h-96 bg-blue-600/10 rounded-full blur-3xl pointer-events-none" />

              <div className="relative z-10 max-w-3xl space-y-6">
                <div className="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full bg-cyan-500/10 border border-cyan-500/30 text-cyan-400 text-xs font-semibold">
                  <Sparkles className="w-3.5 h-3.5" />
                  <span>{t.hero.badge}</span>
                </div>

                <h1 className="text-3xl sm:text-5xl font-black text-white leading-tight">
                  {t.hero.title}
                </h1>

                <p className="text-slate-400 text-base sm:text-lg leading-relaxed">
                  {t.hero.subtitle}
                </p>

                <div className="flex flex-wrap gap-4 pt-4">
                  <button
                    onClick={() => setActiveTab('store')}
                    className="px-6 py-3.5 rounded-xl bg-gradient-to-r from-cyan-500 to-blue-600 text-slate-950 font-bold shadow-lg shadow-cyan-500/25 hover:brightness-110 transition flex items-center gap-2"
                  >
                    <ShoppingBag className="w-5 h-5" />
                    <span>{t.hero.ctaStore}</span>
                  </button>

                  <button
                    onClick={() => setActiveTab('reports')}
                    className="px-6 py-3.5 rounded-xl bg-slate-800/80 hover:bg-slate-800 text-white font-medium border border-slate-700 transition flex items-center gap-2"
                  >
                    <FileText className="w-5 h-5 text-cyan-400" />
                    <span>{t.hero.ctaReports}</span>
                  </button>
                </div>
              </div>
            </div>

            {/* LIVE INTELLIGENCE METRICS */}
            <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
              {[
                { label: lang === 'ar' ? 'تقارير دولية ملخصة' : 'Summarized Reports', val: '120+', icon: BookOpen },
                { label: lang === 'ar' ? 'أدلة رقمية جاهزة' : 'Digital Toolkits', val: '45+', icon: Download },
                { label: lang === 'ar' ? 'دقة التمحيص بالذكاء الاصطناعي' : 'AI Verification Rate', val: '99.4%', icon: Shield },
                { label: lang === 'ar' ? 'تغطية عالمية مزدوجة' : 'Bilingual Reach', val: 'AR / EN', icon: Globe }
              ].map((item, idx) => (
                <div key={idx} className="p-5 rounded-2xl bg-slate-900/50 border border-slate-800/80 flex items-center gap-4">
                  <div className="p-3 rounded-xl bg-cyan-500/10 text-cyan-400">
                    <item.icon className="w-6 h-6" />
                  </div>
                  <div>
                    <span className="text-2xl font-black text-white">{item.val}</span>
                    <span className="block text-xs text-slate-400 mt-0.5">{item.label}</span>
                  </div>
                </div>
              ))}
            </div>

            {/* FEATURED DIGITAL PRODUCTS SECTION */}
            <div className="space-y-6">
              <div className="flex items-center justify-between">
                <div>
                  <h2 className="text-2xl font-bold text-white">
                    {lang === 'ar' ? 'أهم المنتجات والأدلة الرقمية' : 'Featured Digital Products'}
                  </h2>
                  <p className="text-xs text-slate-400 mt-1">
                    {lang === 'ar' ? 'أدوات وحقائب جاهزة للبيع والتنزيل المباشر' : 'Income-generating, ready-to-download toolkits'}
                  </p>
                </div>

                <button 
                  onClick={() => setActiveTab('store')}
                  className="text-xs font-semibold text-cyan-400 hover:text-cyan-300 flex items-center gap-1"
                >
                  <span>{lang === 'ar' ? 'عرض الكل' : 'View All'}</span>
                  <ArrowRight className="w-4 h-4" />
                </button>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                {products.slice(0, 4).map((prod) => (
                  <div key={prod.id} className="rounded-2xl bg-slate-900 border border-slate-800 p-5 flex flex-col justify-between hover:border-slate-700 transition group">
                    <div className="space-y-3">
                      <div className="flex items-center justify-between">
                        <span className="text-[10px] uppercase tracking-wider px-2.5 py-1 rounded-md bg-slate-800 text-cyan-400 font-semibold">
                          {prod.category}
                        </span>
                        <span className="text-xs font-bold text-amber-400 flex items-center gap-1">
                          <Star className="w-3.5 h-3.5 fill-amber-400" />
                          {prod.rating}
                        </span>
                      </div>

                      <h3 className="font-bold text-white text-sm line-clamp-2 group-hover:text-cyan-400 transition">
                        {prod.title[lang]}
                      </h3>

                      <p className="text-slate-400 text-xs line-clamp-2">
                        {prod.description[lang]}
                      </p>
                    </div>

                    <div className="pt-4 border-t border-slate-800/80 mt-4 flex items-center justify-between">
                      <div>
                        <span className="text-xs text-slate-500 block">{prod.format}</span>
                        <span className="text-lg font-black text-white">
                          {currency === 'USD' ? `$${prod.priceUSD}` : `${prod.priceMAD} MAD`}
                        </span>
                      </div>

                      <button
                        onClick={() => addToCart(prod)}
                        className="p-2.5 rounded-xl bg-cyan-500/10 hover:bg-cyan-500 text-cyan-400 hover:text-slate-950 transition"
                      >
                        <ShoppingBag className="w-4 h-4" />
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            </div>

          </div>
        )}

        {/* ========================================== */}
        {/* TAB 2: GLOBAL REPORTS & EXECUTIVE SUMMARIES*/}
        {/* ========================================== */}
        {activeTab === 'reports' && (
          <div className="space-y-8">
            <div className="border-b border-slate-800 pb-6">
              <h1 className="text-3xl font-black text-white">
                {lang === 'ar' ? 'مركز التقارير والدراسات الدولية' : 'Global Reports & Executive Summaries'}
              </h1>
              <p className="text-slate-400 text-sm mt-2">
                {lang === 'ar' ? 'دراسات دقيقة وممخصة بأسلوب سليم لمعرفة التغيرات التقنية ومستقبل العمل.' : 'Vetted, high-precision research summaries tracking technological shifts & the future of work.'}
              </p>
            </div>

            {/* Category Filter Pills */}
            <div className="flex flex-wrap gap-2">
              {Object.keys(t.categories).map((catKey) => (
                <button
                  key={catKey}
                  onClick={() => setSelectedCategory(catKey)}
                  className={`px-4 py-2 rounded-xl text-xs font-semibold transition ${
                    selectedCategory === catKey
                      ? 'bg-cyan-500 text-slate-950'
                      : 'bg-slate-900 text-slate-400 border border-slate-800 hover:border-slate-700'
                  }`}
                >
                  {t.categories[catKey]}
                </button>
              ))}
            </div>

            {/* Reports List */}
            <div className="space-y-6">
              {filteredReports.map((rep) => (
                <div key={rep.id} className="p-6 rounded-2xl bg-slate-900 border border-slate-800 hover:border-slate-700 transition space-y-4">
                  <div className="flex flex-wrap items-center justify-between gap-2">
                    <div className="flex items-center gap-2">
                      <span className="px-2.5 py-1 rounded-md bg-cyan-500/10 text-cyan-400 text-xs font-bold">
                        {rep.category}
                      </span>
                      <span className="text-xs text-slate-500">• {rep.source}</span>
                    </div>
                    <span className="text-xs text-slate-500">{rep.date}</span>
                  </div>

                  <h2 className="text-xl font-bold text-white">
                    {rep.title[lang]}
                  </h2>

                  <p className="text-slate-300 text-sm leading-relaxed">
                    {rep.summary[lang]}
                  </p>

                  <div className="p-4 rounded-xl bg-slate-950/60 border border-slate-800/60 space-y-2">
                    <span className="text-xs font-bold text-cyan-400 block uppercase tracking-wider">
                      {lang === 'ar' ? 'أبرز الاستنتاجات التنفيذية:' : 'Key Executive Takeaways:'}
                    </span>
                    <ul className="space-y-1">
                      {rep.keyTakeaways.map((item, i) => (
                        <li key={i} className="text-xs text-slate-400 flex items-center gap-2">
                          <CheckCircle className="w-3.5 h-3.5 text-cyan-400 flex-shrink-0" />
                          <span>{item[lang]}</span>
                        </li>
                      ))}
                    </ul>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* TAB 3: DIGITAL STOREFRONT & TOOLKITS       */}
        {/* ========================================== */}
        {activeTab === 'store' && (
          <div className="space-y-8">
            <div className="border-b border-slate-800 pb-6 flex flex-wrap items-center justify-between gap-4">
              <div>
                <h1 className="text-3xl font-black text-white">
                  {lang === 'ar' ? 'متجر المنتجات والأدلة الرقمية' : 'Digital Products & Toolkits Store'}
                </h1>
                <p className="text-slate-400 text-sm mt-2">
                  {lang === 'ar' ? 'أدلة وقوائم عمل قابلة للشراء والتنزيل الفوري لتأمين الإنتاجية والدخل.' : 'Actionable toolkits & digital guides for immediate download and application.'}
                </p>
              </div>

              {/* Currency Selector */}
              <div className="flex items-center gap-2 bg-slate-900 p-1.5 rounded-xl border border-slate-800 text-xs">
                <span className="text-slate-400 px-2">{lang === 'ar' ? 'العملة:' : 'Currency:'}</span>
                <button
                  onClick={() => setCurrency('USD')}
                  className={`px-3 py-1 rounded-lg font-bold ${currency === 'USD' ? 'bg-cyan-500 text-slate-950' : 'text-slate-400'}`}
                >
                  USD ($)
                </button>
                <button
                  onClick={() => setCurrency('MAD')}
                  className={`px-3 py-1 rounded-lg font-bold ${currency === 'MAD' ? 'bg-cyan-500 text-slate-950' : 'text-slate-400'}`}
                >
                  MAD (درهم)
                </button>
              </div>
            </div>

            {/* Products Grid */}
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
              {filteredProducts.map((prod) => (
                <div key={prod.id} className="rounded-2xl bg-slate-900 border border-slate-800 p-6 flex flex-col justify-between hover:border-cyan-500/50 transition group">
                  <div className="space-y-4">
                    <div className="flex items-center justify-between">
                      <span className="text-xs font-bold px-2.5 py-1 rounded-md bg-cyan-500/10 text-cyan-400">
                        {prod.category}
                      </span>
                      <span className="text-xs px-2 py-0.5 rounded bg-amber-500/10 text-amber-400 font-bold">
                        {prod.badge}
                      </span>
                    </div>

                    <h3 className="text-lg font-bold text-white group-hover:text-cyan-400 transition">
                      {prod.title[lang]}
                    </h3>

                    <p className="text-slate-400 text-xs leading-relaxed">
                      {prod.description[lang]}
                    </p>
                  </div>

                  <div className="pt-6 border-t border-slate-800 mt-6 flex items-center justify-between">
                    <div>
                      <span className="text-[10px] text-slate-500 uppercase block">{prod.format}</span>
                      <span className="text-2xl font-black text-white">
                        {currency === 'USD' ? `$${prod.priceUSD}` : `${prod.priceMAD} MAD`}
                      </span>
                    </div>

                    <button
                      onClick={() => addToCart(prod)}
                      className="px-4 py-2.5 rounded-xl bg-gradient-to-r from-cyan-500 to-blue-600 text-slate-950 font-bold text-xs hover:brightness-110 transition flex items-center gap-2"
                    >
                      <ShoppingBag className="w-4 h-4" />
                      <span>{lang === 'ar' ? 'شراء الآن' : 'Buy Now'}</span>
                    </button>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* TAB 4: ACADEMY & COURSES                   */}
        {/* ========================================== */}
        {activeTab === 'academy' && (
          <div className="space-y-8">
            <div className="border-b border-slate-800 pb-6">
              <h1 className="text-3xl font-black text-white">
                {lang === 'ar' ? 'أكاديمية Futurify للمهارات الرقمية' : 'Futurify Digital Skills Academy'}
              </h1>
              <p className="text-slate-400 text-sm mt-2">
                {lang === 'ar' ? 'مسارات تدريبية احترافية لإتقان مهارات المستقبل وتطبيق الذكاء الاصطناعي في بيئة العمل.' : 'Professional learning tracks mastering future skills & AI workplace implementation.'}
              </p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              {[
                {
                  title: { ar: 'دورة هندسة الحوافز وتصميم تطبيقات الذكاء الاصطناعي', en: 'Prompt Engineering & AI Application Design Course' },
                  duration: '8 Hours • Certified',
                  level: 'Intermediate',
                  desc: { ar: 'تدريب عملي على بناء مساعدين ذكيين وأتمتة المهام المعقدة بالمؤسسة.', en: 'Hands-on training building custom AI agents and automating complex tasks.' }
                },
                {
                  title: { ar: 'مسار التحول الرقمي وأمن المعلومات للقياديين', en: 'Digital Transformation & Cybersecurity Executive Track' },
                  duration: '12 Hours • Certified',
                  level: 'Executive',
                  desc: { ar: 'إعداد القيادات الإدارية لإدارة مخاطر التكنولوجيا وبناء استراتيجيات آمنة.', en: 'Equipping leaders to manage tech risks and deploy resilient digital strategies.' }
                }
              ].map((course, idx) => (
                <div key={idx} className="p-6 rounded-2xl bg-slate-900 border border-slate-800 space-y-4">
                  <div className="flex items-center justify-between text-xs text-cyan-400 font-semibold">
                    <span>{course.duration}</span>
                    <span className="px-2 py-0.5 rounded bg-slate-800">{course.level}</span>
                  </div>
                  <h3 className="text-xl font-bold text-white">{course.title[lang]}</h3>
                  <p className="text-slate-400 text-xs leading-relaxed">{course.desc[lang]}</p>
                  <button 
                    onClick={() => showToast(lang === 'ar' ? 'سيتم فتح باب التسجيل قريباً' : 'Registration opening soon')}
                    className="w-full py-3 rounded-xl bg-slate-800 hover:bg-slate-700 text-white font-bold text-xs transition"
                  >
                    {lang === 'ar' ? 'الانضمام لقائمة الانتظار' : 'Join Waitlist'}
                  </button>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* TAB 5: ADMIN & CURATION DASHBOARD          */}
        {/* ========================================== */}
        {activeTab === 'admin' && (
          <div className="space-y-8">
            <div className="border-b border-slate-800 pb-6 flex items-center justify-between">
              <div>
                <h1 className="text-3xl font-black text-white">
                  {lang === 'ar' ? 'لوحة الإشراف والتدقيق الذكي' : 'Smart Curation & Admin Dashboard'}
                </h1>
                <p className="text-slate-400 text-sm mt-2">
                  {lang === 'ar' ? 'مراجعة وتدقيق التقارير قبل الاعتماد والنشر الآلي باللغتين.' : 'Review and audit AI-summarized intelligence before automated dual-language publishing.'}
                </p>
              </div>
              <span className="px-3 py-1 rounded-full bg-emerald-500/10 text-emerald-400 border border-emerald-500/20 text-xs font-bold flex items-center gap-1.5">
                <CheckCircle className="w-4 h-4" />
                <span>{lang === 'ar' ? 'النظام جاهز' : 'System Ready'}</span>
              </span>
            </div>

            {/* Create & Audit Report Form */}
            <form onSubmit={handleCreateReport} className="p-6 rounded-2xl bg-slate-900 border border-slate-800 space-y-6">
              <h2 className="text-lg font-bold text-white flex items-center gap-2">
                <Sparkles className="w-5 h-5 text-cyan-400" />
                <span>{lang === 'ar' ? 'تدقيق اعتماد تقرير جديد' : 'Audit & Publish New Report'}</span>
              </h2>

              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div className="space-y-2">
                  <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'العنوان (بالعربية)' : 'Title (Arabic)'}</label>
                  <input
                    type="text"
                    value={newReportTitleAr}
                    onChange={(e) => setNewReportTitleAr(e.target.value)}
                    placeholder="مثال: تقرير الذكاء الاصطناعي 2026..."
                    className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none"
                    required
                  />
                </div>

                <div className="space-y-2">
                  <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'العنوان (بالإنجليزية)' : 'Title (English)'}</label>
                  <input
                    type="text"
                    value={newReportTitleEn}
                    onChange={(e) => setNewReportTitleEn(e.target.value)}
                    placeholder="e.g. AI Trends Report 2026..."
                    className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none"
                    required
                  />
                </div>
              </div>

              <div className="space-y-2">
                <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'المجال' : 'Category'}</label>
                <select
                  value={newReportCategory}
                  onChange={(e) => setNewReportCategory(e.target.value)}
                  className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none"
                >
                  <option value="AI & Automation">AI & Automation</option>
                  <option value="Cybersecurity">Cybersecurity</option>
                  <option value="Data Analytics">Data Analytics</option>
                  <option value="Productivity">Productivity</option>
                </select>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div className="space-y-2">
                  <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'الملخص التنفيذي (بالعربية)' : 'Executive Summary (Arabic)'}</label>
                  <textarea
                    rows={3}
                    value={newReportSummaryAr}
                    onChange={(e) => setNewReportSummaryAr(e.target.value)}
                    className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none resize-none"
                  />
                </div>

                <div className="space-y-2">
                  <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'الملخص التنفيذي (بالإنجليزية)' : 'Executive Summary (English)'}</label>
                  <textarea
                    rows={3}
                    value={newReportSummaryEn}
                    onChange={(e) => setNewReportSummaryEn(e.target.value)}
                    className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none resize-none"
                  />
                </div>
              </div>

              <button
                type="submit"
                className="w-full py-3.5 rounded-xl bg-gradient-to-r from-cyan-500 to-blue-600 text-slate-950 font-bold text-sm hover:brightness-110 transition flex items-center justify-center gap-2"
              >
                <CheckCircle className="w-5 h-5" />
                <span>{lang === 'ar' ? 'اعتماد ونشر التقرير فوراً' : 'Approve & Publish Report'}</span>
              </button>
            </form>
          </div>
        )}

        {/* ========================================== */}
        {/* TAB 6: CONTACT & NETWORK HUB              */}
        {/* ========================================== */}
        {activeTab === 'contact' && (
          <div className="max-w-2xl mx-auto space-y-8">
            <div className="border-b border-slate-800 pb-6 text-center">
              <h1 className="text-3xl font-black text-white">
                {lang === 'ar' ? 'مركز التواصل والدعم' : 'Contact & Network Hub'}
              </h1>
              <p className="text-slate-400 text-sm mt-2">
                {lang === 'ar' ? 'يسعدنا تواصلكم للاستفسارات الشراكات والتوزيع الشبكي.' : 'Reach out for inquiries, distribution networks, and partnerships.'}
              </p>
            </div>

            <form 
              onSubmit={(e) => {
                e.preventDefault();
                showToast(lang === 'ar' ? 'تم إرسال رسالتك بنجاح!' : 'Message sent successfully!');
              }} 
              className="p-6 rounded-2xl bg-slate-900 border border-slate-800 space-y-4"
            >
              <div className="space-y-2">
                <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'الاسم الكامل' : 'Full Name'}</label>
                <input type="text" required className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none" />
              </div>

              <div className="space-y-2">
                <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'البريد الإلكتروني' : 'Email Address'}</label>
                <input type="email" required className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none" />
              </div>

              <div className="space-y-2">
                <label className="text-xs font-semibold text-slate-400 block">{lang === 'ar' ? 'الرسالة' : 'Message'}</label>
                <textarea rows={4} required className="w-full px-4 py-2.5 rounded-xl bg-slate-950 border border-slate-800 text-white text-xs focus:border-cyan-500 outline-none resize-none" />
              </div>

              <button type="submit" className="w-full py-3.5 rounded-xl bg-cyan-500 text-slate-950 font-bold text-xs hover:bg-cyan-400 transition">
                {lang === 'ar' ? 'إرسال الرسالة' : 'Send Message'}
              </button>
            </form>
          </div>
        )}

      </main>

      {/* FOOTER */}
      <footer className="border-t border-slate-900 bg-slate-950 py-8 mt-16 text-center text-xs text-slate-500">
        <div className="max-w-7xl mx-auto px-4">
          <p>© 2026 {t.brand}. {lang === 'ar' ? 'جميع الحقوق محفوظة. منصة المعرفة والتمكين الرقمي.' : 'All rights reserved. Digital Empowerment Platform.'}</p>
        </div>
      </footer>

    </div>
  );
}
