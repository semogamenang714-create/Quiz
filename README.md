<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>J-MASTER SRS - Desti</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .perspective { perspective: 1000px; }
        .card-inner {
            transition: transform 0.6s;
            transform-style: preserve-3d;
            cursor: pointer;
        }
        .is-flipped { transform: rotateY(180deg); }
        .card-front, .card-back {
            backface-visibility: hidden;
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
        }
        .card-back { transform: rotateY(180deg); }
    </style>
</head>
<body class="bg-slate-50 min-h-screen p-4 flex flex-col items-center">

    <div class="w-full max-w-sm flex flex-col">
        <div class="flex justify-between items-center mb-6">
            <div>
                <h1 class="text-xl font-black tracking-tighter">J-MASTER <span class="text-indigo-600 underline decoration-2 underline-offset-4">SRS</span></h1>
                <p class="text-[10px] font-bold text-slate-400 mt-1 uppercase tracking-widest">Persiapan Kerja di Jepang</p>
            </div>
            <div class="text-xs bg-white px-3 py-1 rounded-full shadow-sm border border-slate-100 font-bold text-indigo-500">
                JLPT N3 / SSW
            </div>
        </div>

        <div id="study-view" class="space-y-6">
            <div class="flex justify-between items-center px-1">
                <span id="counter" class="text-[10px] font-black text-indigo-400 bg-indigo-50 px-3 py-1 rounded-full uppercase">Memuat Kartu...</span>
            </div>

            <div class="perspective h-80 w-full">
                <div id="flashcard" class="card-inner relative w-full h-full" onclick="flipCard()">
                    <div class="card-front bg-white rounded-[2.5rem] shadow-sm border border-slate-100 flex flex-col items-center justify-center p-6">
                        <span class="text-[9px] font-black text-slate-300 uppercase tracking-widest mb-6">Kanji / Kata</span>
                        <div id="card-front-text" class="text-6xl font-bold text-slate-800 tracking-tighter">一</div>
                        <div class="absolute bottom-10 text-slate-200 text-[10px] font-bold uppercase animate-pulse">Tap untuk arti</div>
                    </div>
                    <div class="card-back bg-white rounded-[2.5rem] shadow-md border-2 border-indigo-50 flex flex-col items-center justify-center p-6">
                        <span class="text-[9px] font-black text-indigo-300 uppercase tracking-widest mb-2">Arti & Cara Baca</span>
                        <div id="card-reading" class="text-lg text-indigo-500 font-bold mb-1 tracking-tight">いち</div>
                        <div id="card-meaning" class="text-2xl font-bold text-slate-800 text-center">Satu</div>
                    </div>
                </div>
            </div>

            <div id="controls" class="grid grid-cols-2 gap-3 opacity-0 pointer-events-none transition-all duration-300">
                <button onclick="nextCard(false)" class="py-4 rounded-2xl bg-orange-50 font-black text-[10px] text-orange-500 border border-orange-100 uppercase tracking-widest border-b-4 active:border-b-0 active:translate-y-1">Belum Hafal</button>
                <button onclick="nextCard(true)" class="py-4 rounded-2xl bg-indigo-600 font-black text-[10px] text-white uppercase tracking-widest border-b-4 border-indigo-800 active:border-b-0 active:translate-y-1">Lulus (Hafal)</button>
            </div>
        </div>

        <div id="finish-view" class="hidden flex flex-col items-center justify-center py-10 text-center animate-bounce-slow">
            <div class="text-6xl mb-4">🎉</div>
            <h2 class="text-xl font-bold text-slate-800">Selesai untuk Hari Ini!</h2>
            <p class="text-slate-500 text-sm mt-2">Bagus sekali, Desti! Satu langkah lebih dekat menuju Jepang.</p>
            <button onclick="location.reload()" class="mt-8 text-indigo-600 font-bold text-xs uppercase tracking-widest border-b-2 border-indigo-200">Ulang Sesi</button>
        </div>
    </div>

    <script>
        // Data Kanji N5 Sederhana untuk contoh
        const kanjiData = [
            { front: '一', reading: 'いち', meaning: 'Satu' },
            { front: '二', reading: 'に', meaning: 'Dua' },
            { front: '三', reading: 'さん', meaning: 'Tiga' },
            { front: '人', reading: 'ひと / じん', meaning: 'Orang' },
            { front: '水', reading: 'みず', meaning: 'Air' },
            { front: '日本', reading: 'にほん', meaning: 'Jepang' },
            { front: '私', reading: 'わたし', meaning: 'Saya' }
        ];

        let currentIndex = 0;
        let cards = [...kanjiData].sort(() => Math.random() - 0.5);

        function updateUI() {
            if (currentIndex >= cards.length) {
                document.getElementById('study-view').classList.add('hidden');
                document.getElementById('finish-view').classList.remove('hidden');
                return;
            }

            const card = cards[currentIndex];
            document.getElementById('card-front-text').innerText = card.front;
            document.getElementById('card-reading').innerText = card.reading;
            document.getElementById('card-meaning').innerText = card.meaning;
            document.getElementById('counter').innerText = `Kartu: ${currentIndex + 1} / ${cards.length}`;
            
            // Reset card state
            document.getElementById('flashcard').classList.remove('is-flipped');
            document.getElementById('controls').classList.add('opacity-0', 'pointer-events-none');
        }

        function flipCard() {
            document.getElementById('flashcard').classList.add('is-flipped');
            document.getElementById('controls').classList.remove('opacity-0', 'pointer-events-none');
        }

        function nextCard(isEasy) {
            if (!isEasy) {
                // Kalau susah, masukkan lagi ke urutan paling belakang biar muncul lagi
                const currentCard = cards[currentIndex];
                cards.push(currentCard);
            }
            currentIndex++;
            updateUI();
        }

        // Jalankan saat pertama kali dibuka
        updateUI();
    </script>
</body>
</html>
