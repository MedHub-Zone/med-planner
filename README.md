<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MedHub Pro | تحديث رمضان التلقائي</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        body { background: #020617; color: white; font-family: 'Cairo', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(251, 191, 36, 0.1); border-radius: 24px; }
        .check-item { appearance: none; width: 20px; height: 20px; border: 2px solid #fbbf24; border-radius: 50%; cursor: pointer; position: relative; }
        .check-item:checked { background: #fbbf24; }
        .hidden { display: none; }
    </style>
</head>
<body class="p-4 md:p-8">

    <div id="main-menu" class="max-w-6xl mx-auto mt-10 text-center">
        <h1 class="text-4xl font-bold mb-4 text-amber-400">MedHub Pro</h1>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div onclick="showPage('planner-page')" class="glass p-8 cursor-pointer border-amber-500/30">
                <i class="fas fa-moon text-5xl text-amber-400 mb-4"></i>
                <h2 class="text-xl font-bold">نوتة رمضان</h2>
            </div>
            <div onclick="showPage('timer-page')" class="glass p-8 cursor-pointer">
                <i class="fas fa-stopwatch text-5xl text-sky-400 mb-4"></i>
                <h2 class="text-xl font-bold">المؤقت</h2>
            </div>
            <div onclick="showPage('medical-page')" class="glass p-8 cursor-pointer">
                <i class="fas fa-heartbeat text-5xl text-red-500 mb-4"></i>
                <h2 class="text-xl font-bold">Health Stats</h2>
            </div>
        </div>
    </div>

    <div id="planner-page" class="hidden max-w-6xl mx-auto">
        <button onclick="showPage('main-menu')" class="text-amber-400 mb-6 font-bold">← العودة للرئيسية</button>
        
        <div class="glass p-6 md:p-10 border border-amber-500/20">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-10 border-b border-amber-500/10 pb-8">
                <div>
                    <h2 id="today-date" class="text-2xl font-bold text-amber-400">...</h2>
                    <p id="daily-ayah" class="italic mt-3 text-amber-100/80 text-lg leading-relaxed">"جارِ تحميل آية اليوم..."</p>
                </div>
                
                <div class="bg-amber-900/20 p-4 rounded-3xl border border-amber-500/30 text-center">
                    <span class="block text-xs text-amber-200 mb-1">سبحة الأذكار</span>
                    <div id="tasbih-count" class="text-3xl font-bold text-amber-400">0</div>
                    <button onclick="countTasbih()" class="bg-amber-500 text-black px-6 py-1 rounded-full font-bold mt-2 text-sm">تسبِيح</button>
                </div>

                <div class="flex flex-wrap gap-2 justify-center items-center">
                    <span class="p-3 glass text-2xl">🌙</span>
                    <span class="p-3 glass text-2xl">🕌</span>
                    <span class="p-3 glass text-2xl">📿</span>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-10">
                <div class="space-y-8">
                    <section>
                        <h3 class="text-amber-400 font-bold mb-4 flex items-center gap-2"><i class="fas fa-book-quran"></i> الورد القرآني والصيام</h3>
                        <div class="glass p-4 space-y-3">
                            <label class="flex items-center gap-3"><input type="checkbox" class="check-item"> نويت الصيام</label>
                            <label class="flex items-center gap-3"><input type="checkbox" class="check-item"> قراءة ورد اليوم</label>
                            <input type="text" placeholder="وصلت للجزء / الصفحة رقم..." class="bg-transparent border-b border-amber-500/20 w-full p-2 outline-none text-sm italic">
                        </div>
                    </section>

                    <section>
                        <h3 class="text-amber-400 font-bold mb-4 flex items-center gap-2"><i class="fas fa-tasks"></i> مهام المذاكرة</h3>
                        <div id="tasks-container" class="space-y-3">
                            <div class="flex items-center gap-3 border-b border-white/5 pb-2">
                                <input type="checkbox" class="check-item">
                                <input type="text" placeholder="اكتبي مهمة جديدة..." class="bg-transparent w-full outline-none text-white">
                            </div>
                        </div>
                        <button onclick="addRow()" class="text-amber-500 mt-4 text-sm">+ إضافة مهمة</button>
                    </section>
                </div>

                <div class="space-y-8">
                    <div class="bg-amber-500/5 p-6 rounded-3xl border border-amber-500/20">
                        <h4 class="text-amber-400 font-bold mb-3 italic">نصيحة اليوم:</h4>
                        <p id="daily-tip" class="text-gray-300 leading-relaxed">جارِ اختيار نصيحة اليوم...</p>
                    </div>

                    <div class="glass p-6">
                        <h4 class="text-amber-400 font-bold mb-4 underline">أذكار لا تنسيها:</h4>
                        <ul class="text-sm space-y-2 opacity-80">
                            <li>• سبحان الله وبحمده (100 مرة)</li>
                            <li>• أستغفر الله وأتوب إليه</li>
                            <li>• اللهم صلِّ وسلم على نبينا محمد</li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // مصفوفة الآيات والنصائح (تتغير تلقائياً كل يوم)
        const content = {
            ayahs: [
                "وَسَارِعُوا إِلَى مَغْفِرَةٍ مِّن رَّبِّكُمْ وَجَنَّةٍ عَرْضُهَا السَّمَاوَاتُ وَالْأَرْضُ",
                "لَيْلَةُ الْقَدْرِ خَيْرٌ مِّنْ أَلْفِ شَهْرٍ",
                "وَتزوَّدوا فَإِنَّ خَيْرَ الزَّادِ التَّقْوَىٰ",
                "أَيَّامًا مَّعْدُودَاتٍ ۚ فَمَن كَانَ مِنكُم مَّرِيضًا أَوْ عَلَىٰ سَفَرٍ فَعِدَّةٌ مِّنْ أَيَّامٍ أُخَرَ"
            ],
            tips: [
                "اشربي كمية كافية من الماء بين الفطور والسحور لتجنب الصداع أثناء المذاكرة.",
                "أفضل وقت لمذاكرة المواد الصعبة هو بعد صلاة الفجر مباشرة.",
                "لا تنسي أخذ قيلولة قصيرة (30 دقيقة) قبل صلاة العصر لتجديد نشاطك.",
                "اجعلي مذاكرتك بنية العبادة، فطلب العلم فريضة وأجرها مضاعف في رمضان."
            ]
        };

        function updateDailyContent() {
            const dayOfYear = Math.floor(new Date() / 8.64e7) % content.ayahs.length;
            document.getElementById('daily-ayah').innerText = `"${content.ayahs[dayOfYear]}"`;
            document.getElementById('daily-tip').innerText = content.tips[dayOfYear];
        }

        function showPage(id) {
            document.querySelectorAll('[id$="-page"], #main-menu').forEach(p => p.classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        }

        let c = 0;
        function countTasbih() { c++; document.getElementById('tasbih-count').innerText = c; }

        function addRow() {
            const div = document.createElement('div');
            div.className = "flex items-center gap-3 border-b border-white/5 pb-2";
            div.innerHTML = '<input type="checkbox" class="check-item"><input type="text" class="bg-transparent w-full outline-none text-white">';
            document.getElementById('tasks-container').appendChild(div);
        }

        document.getElementById('today-date').innerText = new Date().toLocaleDateString('ar-EG', {day:'numeric', month:'long'});
        updateDailyContent();
    </script>
</body>
</html>
