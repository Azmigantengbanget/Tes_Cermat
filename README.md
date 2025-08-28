<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [



  { "en": "OOOOOOOO0OOOOOO. Simbol yang berbeda adalah?", "id": "0" },
  { "en": "MMMMMMMMWMMMMMM. Huruf yang berbeda adalah?", "id": "W" },
  { "en": "8888888B8888888. Simbol yang berbeda adalah?", "id": "B" },
  { "en": "Carilah perbedaan: qqqqqpqqqqq?", "id": "p" },
  { "en": "Manakah yang tidak sama: bbbbbdbbbbb?", "id": "d" },
  { "en": "Sebutkan angka yang berbeda: 999999699999?", "id": "6" },
  { "en": "Temukan huruf yang ganjil: EEEEEFEEEEE?", "id": "F" },
  { "en": "Lihat deret ini: IIIIIIlIIIIII. Simbol beda?", "id": "l" },
  { "en": "Mana yang salah: ZZZZZZZ2ZZZZZ?", "id": "2" },
  { "en": "Tunjukkan simbol beda: &&&&&%&&&&&?", "id": "%" },
  { "en": "Bandingkan: (ABCDE) dan (ABCD_). Huruf hilang?", "id": "E" },
  { "en": "Bandingkan: (12345) dan (12_45). Angka hilang?", "id": "3" },
  { "en": "Lengkapi urutan: P Q R S _ U V?", "id": "T" },
  { "en": "Lengkapi urutan: 10 20 30 _ 50?", "id": "40" },
  { "en": "Huruf apa yang hilang: K L M N O _ Q?", "id": "P" },
  { "en": "Sama atau beda: (KLMNO) vs (KLMN0)?", "id": "Beda, O dan 0." },
  { "en": "Sama atau beda: (qwerty) vs (qwertu)?", "id": "Beda, y dan u." },
  { "en": "Sama atau beda: (867530) vs (867530)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (mnbvcx) vs (nbmvcx)?", "id": "Beda urutan." },
  { "en": "Sama atau beda: (1111l1) vs (111111)?", "id": "Beda, l dan 1." },
  { "en": "Kata yang salah: Mobil, Mobil, Mobli, Mobil?", "id": "Mobli" },
  { "en": "Kata yang salah: Kucing, Kucing, Kucing, Kucingg?", "id": "Kucingg" },
  { "en": "Kata yang salah: Indonesia, Indoneisa, Indonesia?", "id": "Indoneisa" },
  { "en": "Kata yang salah: Jakarta, Jakarta, Jakrta, Jakarta?", "id": "Jakrta" },
  { "en": "Kata yang salah: Merah, Merah, Merha, Merah?", "id": "Merha" },
  { "en": "Berapa huruf 'a' dalam 'matematika'?", "id": "5" },
  { "en": "Berapa angka '9' dalam '989796959'?", "id": "5" },
  { "en": "Berapa huruf 'm' dalam 'memasak mie'?", "id": "3" },
  { "en": "Berapa angka '1' dalam '100110101'?", "id": "5" },
  { "en": "Berapa huruf 'b' dalam 'bola bekel biru'?", "id": "3" },
  { "en": "Cari huruf beda: gggggggggqggggg?", "id": "q" },
  { "en": "Cari angka beda: 55555S55555?", "id": "S" },
  { "en": "Cari simbol beda: @@@@@@&@@@@@@?", "id": "&" },
  { "en": "Cari huruf beda: uuuuuuvuuuuuu?", "id": "v" },
  { "en": "Cari angka beda: 22222Z22222?", "id": "Z" },
  { "en": "Bandingkan: (1O1O1O) dan (101010)?", "id": "Beda, O dan 0." },
  { "en": "Bandingkan: (uvwxyz) dan (uvw_yz). Huruf hilang?", "id": "x" },
  { "en": "Lengkapi urutan: Z Y X W _ U?", "id": "V" },
  { "en": "Lengkapi urutan: 9 8 7 _ 5 4?", "id": "6" },
  { "en": "Huruf apa yang hilang: C D E _ G H?", "id": "F" },
  { "en": "Sama atau beda: (abcdef) vs (abcde f)?", "id": "Beda, ada spasi." },
  { "en": "Sama atau beda: (908765) vs (908765)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (IlIlIl) vs (111111)?", "id": "Beda, I, l, dan 1." },
  { "en": "Sama atau beda: (Surabaya) vs (Surabya)?", "id": "Beda, a hilang." },
  { "en": "Sama atau beda: (zxcvbn) vs (zxcvbnm)?", "id": "Beda, m ditambahkan." },
  { "en": "Kata yang salah: Psikotes, Psokotes, Psikotes?", "id": "Psokotes" },
  { "en": "Kata yang salah: Telepon, Telepon, Telpon, Telepon?", "id": "Telpon" },
  { "en": "Kata yang salah: Februari, Februari, Pebruari?", "id": "Pebruari" },
  { "en": "Kata yang salah: Kualitas, Kwalitas, Kualitas?", "id": "Kwalitas" },
  { "en": "Kata yang salah: Sistem, Sistim, Sistem, Sistem?", "id": "Sistim" },
  { "en": "Berapa huruf 'i' dalam 'informasi digital'?", "id": "4" },
  { "en": "Berapa angka '3' dalam '333666333999'?", "id": "6" },
  { "en": "Berapa huruf 'p' dalam 'papan pengumuman'?", "id": "3" },
  { "en": "Berapa angka '0' dalam '100200300400'?", "id": "8" },
  { "en": "Berapa huruf 's' dalam 'status seleksi siswa'?", "id": "6" },
  { "en": "Cari huruf beda: ccccccccccCcccc?", "id": "C" },
  { "en": "Cari angka beda: 44444A44444?", "id": "A" },
  { "en": "Cari simbol beda: $$$$$$$S$$$$$?", "id": "S" },
  { "en": "Cari huruf beda: hhhhhhhhhnhhhhh?", "id": "n" },
  { "en": "Cari angka beda: 77777T77777?", "id": "T" },
  { "en": "Bandingkan: (axbycz) dan (axb_cz). Huruf hilang?", "id": "y" },
  { "en": "Bandingkan: (987654) dan (98_654). Angka hilang?", "id": "7" },
  { "en": "Lengkapi urutan: G H I J _ L?", "id": "K" },
  { "en": "Lengkapi urutan: 55 60 65 _ 75?", "id": "70" },
  { "en": "Angka apa yang hilang: 11 22 33 _ 55?", "id": "44" },
  { "en": "Sama atau beda: (vvvvvv) vs (vvvwvv)?", "id": "Beda, ada w." },
  { "en": "Sama atau beda: (555555) vs (5555S5)?", "id": "Beda, ada S." },
  { "en": "Sama atau beda: (POLISI) vs (P0LISI)?", "id": "Beda, O dan 0." },
  { "en": "Sama atau beda: (Bandung) vs (BandunG)?", "id": "Beda, huruf kapital." },
  { "en": "Sama atau beda: (101010) vs (101010)?", "id": "Sama persis." },
  { "en": "Kata yang salah: Analogi, Analogi, Anologi, Analogi?", "id": "Anologi" },
  { "en": "Kata yang salah: Sinonim, Sinonim, Sinomin, Sinonim?", "id": "Sinomin" },
  { "en": "Kata yang salah: Antonim, Antonim, Antonim, Antomim?", "id": "Antomim" },
  { "en": "Kata yang salah: Logika, Logila, Logika, Logika?", "id": "Logila" },
  { "en": "Kata yang salah: Cermat, Cermat, Cermat, Cermta?", "id": "Cermta" },
  { "en": "Berapa huruf 'k' dalam 'kalkulator rusak'?", "id": "3" },
  { "en": "Berapa angka '8' dalam '81828384858'?", "id": "6" },
  { "en": "Berapa huruf 't' dalam 'tes bakat skolastik'?", "id": "4" },
  { "en": "Berapa angka '2' dalam '2244226622'?", "id": "6" },
  { "en": "Berapa huruf 'd' dalam 'dinding berduri'?", "id": "3" },
  { "en": "Cari huruf beda: yyyyyyyyyYyyyyy?", "id": "Y" },
  { "en": "Cari angka beda: 111111I11111?", "id": "I" },
  { "en": "Cari simbol beda: +++++++++-+++++?", "id": "-" },
  { "en": "Cari huruf beda: ffffffffffftfff?", "id": "t" },
  { "en": "Cari angka beda: 66666G66666?", "id": "G" },
  { "en": "Bandingkan: (pqrstuv) dan (pqr_tuv). Huruf hilang?", "id": "s" },
  { "en": "Bandingkan: (56789) dan (5_789). Angka hilang?", "id": "6" },
  { "en": "Lengkapi urutan: A C E G _ K?", "id": "I" },
  { "en": "Lengkapi urutan: 3 6 9 _ 15?", "id": "12" },
  { "en": "Huruf apa yang hilang: U _ S R Q P?", "id": "T" },
  { "en": "Sama atau beda: (asdfghjkl) vs (asdfghjkl)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (67890) vs (6789O)?", "id": "Beda, 0 dan O." },
  { "en": "Sama atau beda: (MANTAP) vs (mantap)?", "id": "Beda, huruf kapital." },
  { "en": "Sama atau beda: (tes-kecermatan) vs (tes kecermatan)?", "id": "Beda, ada tanda hubung." },
  { "en": "Sama atau beda: (1.000) vs (1000)?", "id": "Beda, ada titik." },
  { "en": "Kata yang salah: Komputer, Komputer, Komputel, Komputer?", "id": "Komputel" },
  { "en": "Kata yang salah: Monitor, Monitol, Monitor, Monitor?", "id": "Monitol" },
  { "en": "Kata yang salah: Keyboard, Keyboad, Keyboard, Keyboard?", "id": "Keyboad" },
  { "en": "Kata yang salah: Mouse, Mouse, Mause, Mouse?", "id": "Mause" },
  { "en": "Kata yang salah: Printer, Printer, Printe, Printer?", "id": "Printe" },
  { "en": "Cari yang beda: lIlIlIlIlIlIl?", "id": "I" },
  { "en": "Manakah yang ganjil: SSSSSSSSS5SSSSS?", "id": "5" },
  { "en": "Tunjukkan angka aneh: 666666G666666?", "id": "G" },
  { "en": "Temukan simbol beda: -------_-------?", "id": "_" },
  { "en": "Huruf manakah yang salah: YYYYYYYYYyYYYYY?", "id": "y" },
  { "en": "Angka yang tidak cocok: 444444A444444?", "id": "A" },
  { "en": "Simbol yang berbeda: $$$$$$$B$$$$$$$?", "id": "B" },
  { "en": "Huruf yang tidak sama: HHHHHHHNHHHHHHH?", "id": "N" },
  { "en": "Angka mana yang beda: 00000O00000?", "id": "O" },
  { "en": "Simbol mana yang lain: %%%%%%%%%&%%%%%?", "id": "&" },
  { "en": "Lengkapi pasangan: (QWERTY) - (QWE_TY)?", "id": "R" },
  { "en": "Lengkapi pasangan: (13579) - (13_79)?", "id": "5" },
  { "en": "Teruskan pola ini: B D F H J _ N?", "id": "L" },
  { "en": "Teruskan pola ini: 15 20 25 _ 35?", "id": "30" },
  { "en": "Apa huruf yang hilang: Z X V T _ P?", "id": "R" },
  { "en": "Sama atau beda: (888B888) vs (8888888)?", "id": "Beda, ada B." },
  { "en": "Sama atau beda: (PQRSTU) vs (PQRTSU)?", "id": "Beda, T dan S tertukar." },
  { "en": "Sama atau beda: (987654) vs (987654)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (indonesia) vs (Indonesia)?", "id": "Beda, huruf kapital." },
  { "en": "Sama atau beda: (0O0O0O0) vs (0000000)?", "id": "Beda, ada O." },
  { "en": "Kata yang keliru: Provinsi, Propinsi, Provinsi?", "id": "Propinsi" },
  { "en": "Kata yang keliru: Jadwal, Jadwal, Jadual, Jadwal?", "id": "Jadual" },
  { "en": "Kata yang keliru: Kualitas, Kualitas, Kwalitas?", "id": "Kwalitas" },
  { "en": "Kata yang keliru: Konkret, Konkrit, Konkret, Konkret?", "id": "Konkrit" },
  { "en": "Kata yang keliru: Praktik, Praktik, Praktek, Praktik?", "id": "Praktek" },
  { "en": "Berapa huruf 'u' dalam 'surat keputusan'?", "id": "3" },
  { "en": "Berapa angka '5' dalam '515253545'?", "id": "5" },
  { "en": "Berapa huruf 'n' dalam 'penerimaan nasional'?", "id": "5" },
  { "en": "Berapa angka '7' dalam '777888777999'?", "id": "6" },
  { "en": "Berapa huruf 'g' dalam 'gunung tertinggi'?", "id": "3" },
  { "en": "Tunjukkan yang beda: XXXXXXXXXyXXXXX?", "id": "y" },
  { "en": "Tunjukkan yang ganjil: 333333E333333?", "id": "E" },
  { "en": "Mana simbol yang lain: *********+*****?", "id": "+" },
  { "en": "Mana huruf yang beda: aaaaaaaaaAaaaaa?", "id": "A" },
  { "en": "Mana angka yang salah: 111111l111111?", "id": "l" },
  { "en": "Pasangan hilang: (ZXCVBN) vs (ZXCV_N)?", "id": "B" },
  { "en": "Pasangan hilang: (97531) vs (97_31)?", "id": "5" },
  { "en": "Lengkapi deret: M L K J _ H?", "id": "I" },
  { "en": "Lengkapi deret: 2 4 8 16 _ 64?", "id": "32" },
  { "en": "Huruf apakah yang hilang: T U V W _ Y?", "id": "X" },
  { "en": "Bandingkan: (Database) vs (Data-base)?", "id": "Beda, ada tanda hubung." },
  { "en": "Bandingkan: (1.234) vs (1234)?", "id": "Beda, ada titik." },
  { "en": "Bandingkan: (ASDFG) vs (ASDFG)?", "id": "Sama persis." },
  { "en": "Bandingkan: (qpalzm) vs (qplazm)?", "id": "Beda urutan." },
  { "en": "Bandingkan: (696969) vs (969696)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Apotek, Apotik, Apotek?", "id": "Apotik" },
  { "en": "Ejaan yang salah: Atlet, Atlit, Atlet, Atlet?", "id": "Atlit" },
  { "en": "Ejaan yang salah: Ijazah, Ijasah, Ijazah, Ijazah?", "id": "Ijasah" },
  { "en": "Ejaan yang salah: Nasihat, Nasehat, Nasihat?", "id": "Nasehat" },
  { "en": "Ejaan yang salah: Risiko, Resiko, Risiko, Risiko?", "id": "Resiko" },
  { "en": "Hitung huruf 'e': 'departemen manajemen'?", "id": "5" },
  { "en": "Hitung angka '6': '6162636465'?", "id": "5" },
  { "en": "Hitung huruf 'r': 'struktur organisasi'?", "id": "4" },
  { "en": "Hitung angka '4': '444111444222'?", "id": "6" },
  { "en": "Hitung huruf 'c': 'catatan calon caraka'?", "id": "4" },
  { "en": "Mana yang berbeda: JJJJJJJJjJJJJJJ?", "id": "j" },
  { "en": "Mana yang aneh: 222222S222222?", "id": "S" },
  { "en": "Simbol mana yang ganjil: =====-=====?", "id": "-" },
  { "en": "Huruf mana yang keliru: KKKKKKKkKKKKKK?", "id": "k" },
  { "en": "Angka mana yang salah: 888888B888888?", "id": "B" },
  { "en": "Cari yang hilang: (mnbvcxz) vs (mnbvc_z)?", "id": "x" },
  { "en": "Cari yang hilang: (112233) vs (11_233)?", "id": "2" },
  { "en": "Pola selanjutnya: Z A Y B X _?", "id": "C" },
  { "en": "Pola selanjutnya: 1 3 5 7 _ 11?", "id": "9" },
  { "en": "Huruf apa yang kosong: S R Q P _ N?", "id": "O" },
  { "en": "Lihat pasangan: (GHJKL) vs (G H J K L)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (09876) vs (O9876)?", "id": "Beda, 0 dan O." },
  { "en": "Lihat pasangan: (ACCOUNTING) vs (accounting)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (7777777) vs (7777777)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (lklklk) vs (kikiki)?", "id": "Beda huruf." },
  { "en": "Tulisan salah: November, Nopember, November?", "id": "Nopember" },
  { "en": "Tulisan salah: Izin, Ijin, Izin, Izin?", "id": "Ijin" },
  { "en": "Tulisan salah: Zaman, Jaman, Zaman, Zaman?", "id": "Jaman" },
  { "en": "Tulisan salah: Karier, Karir, Karier?", "id": "Karir" },
  { "en": "Tulisan salah: Objek, Obyek, Objek, Objek?", "id": "Obyek" },
  { "en": "Jumlah huruf 'o': 'otoritas moneter'?", "id": "4" },
  { "en": "Jumlah angka '2': '2123242526'?", "id": "5" },
  { "en": "Jumlah huruf 'a': 'administrasi negara'?", "id": "6" },
  { "en": "Jumlah angka '8': '888777888666'?", "id": "6" },
  { "en": "Jumlah huruf 'l': 'legalitas legal'?", "id": "4" },
  { "en": "Mana yang lain: WWWWWWVWWWWWW?", "id": "V" },
  { "en": "Mana yang beda: 111111T111111?", "id": "T" },
  { "en": "Simbol mana yang aneh: ^^^^^^^&^^^^^?", "id": "&" },
  { "en": "Huruf mana yang salah: pppppppPppppp?", "id": "P" },
  { "en": "Angka mana yang ganjil: 999999g999999?", "id": "g" },
  { "en": "Hilang satu: (zxcvbnm) vs (zxcvb_m)?", "id": "n" },
  { "en": "Hilang satu: (24680) vs (24_80)?", "id": "6" },
  { "en": "Lanjutkan pola: R S T U _ W?", "id": "V" },
  { "en": "Lanjutkan pola: 100 90 80 _ 60?", "id": "70" },
  { "en": "Huruf yang kosong: H G F E _ C?", "id": "D" },
  { "en": "Periksa pasangan: (sama-saja) vs (samasaja)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (9O9O9O) vs (909090)?", "id": "Beda, O dan 0." },
  { "en": "Periksa pasangan: (FINAL) vs (final)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (543210) vs (543210)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (email@) vs (email)?", "id": "Beda, ada simbol." },
  { "en": "Ejaan keliru: Diagnosis, Diagnosa, Diagnosis?", "id": "Diagnosa" },
  { "en": "Ejaan keliru: Standardisasi, Standarisasi, Standardisasi?", "id": "Standarisasi" },
  { "en": "Ejaan keliru: Ekstrem, Ekstrim, Ekstrem?", "id": "Ekstrim" },
  { "en": "Ejaan keliru: Frekuensi, Frekwensi, Frekuensi?", "id": "Frekwensi" },
  { "en": "Ejaan keliru: Hierarki, Hirarki, Hierarki?", "id": "Hirarki" },
  { "en": "Cari yang aneh: PPPPPPPPPbPPPPP?", "id": "b" },
  { "en": "Mana yang berbeda: 7777777T777777?", "id": "T" },
  { "en": "Tunjukkan huruf ganjil: DDDDDDDDDdDDDDD?", "id": "d" },
  { "en": "Temukan simbol beda: ))))))))))(១១១១១?", "id": "(" },
  { "en": "Huruf manakah yang salah: RRRRRRRRrRRRRRR?", "id": "r" },
  { "en": "Angka yang tidak cocok: 333333B333333?", "id": "B" },
  { "en": "Simbol yang berbeda: %%%%%%%%%%%&%%%", "id": "&" },
  { "en": "Huruf yang tidak sama: KKKKKKKkKKKKKKK?", "id": "k" },
  { "en": "Angka mana yang beda: 11111l11111?", "id": "l" },
  { "en": "Simbol mana yang lain: #########@#####?", "id": "@" },
  { "en": "Lengkapi: (ASDFGHJ) - (ASDF_HJ)?", "id": "G" },
  { "en": "Lengkapi: (10203040) - (1020_40)?", "id": "30" },
  { "en": "Lanjutkan pola: A E I M Q _ Y?", "id": "U" },
  { "en": "Lanjutkan pola: 4 8 12 16 _ 24?", "id": "20" },
  { "en": "Apa huruf yang hilang: L K J I _ G?", "id": "H" },
  { "en": "Sama atau beda: (999g999) vs (9999999)?", "id": "Beda, ada g." },
  { "en": "Sama atau beda: (ABCDEFG) vs (ABDCFEG)?", "id": "Beda urutan." },
  { "en": "Sama atau beda: (7654321) vs (7654321)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (SelamatPagi) vs (Selamat Pagi)?", "id": "Beda, tanpa spasi." },
  { "en": "Sama atau beda: (1I1I1I1) vs (1111111)?", "id": "Beda, ada I." },
  { "en": "Kata yang keliru: Aktivitas, Aktifitas, Aktivitas?", "id": "Aktifitas" },
  { "en": "Kata yang keliru: Analisis, Analisa, Analisis?", "id": "Analisa" },
  { "en": "Kata yang keliru: Napas, Nafas, Napas, Napas?", "id": "Nafas" },
  { "en": "Kata yang keliru: Konsekuen, Konsekwen, Konsekuen?", "id": "Konsekwen" },
  { "en": "Kata yang keliru: Kuesioner, Kuisioner, Kuesioner?", "id": "Kuisioner" },
  { "en": "Berapa huruf 'n': 'konstitusi negara'?", "id": "3" },
  { "en": "Berapa angka '4': '414243444'?", "id": "5" },
  { "en": "Berapa huruf 't': 'transportasi publik'?", "id": "3" },
  { "en": "Berapa angka '0': '10100100010000'?", "id": "9" },
  { "en": "Berapa huruf 'p': 'persiapan perlombaan'?", "id": "4" },
  { "en": "Tunjukkan yang beda: GGGGGGGGGgGGGGG?", "id": "g" },
  { "en": "Tunjukkan yang ganjil: 888888S888888?", "id": "S" },
  { "en": "Mana simbol yang lain: (()))((()))?", "id": ")" },
  { "en": "Mana huruf yang beda: BBBBBBBbBBBBB?", "id": "b" },
  { "en": "Mana angka yang salah: 777777l777777?", "id": "l" },
  { "en": "Pasangan hilang: (LKJHGF) vs (LKJH_F)?", "id": "G" },
  { "en": "Pasangan hilang: (123123) vs (123_23)?", "id": "1" },
  { "en": "Lengkapi deret: C F I L _ R?", "id": "O" },
  { "en": "Lengkapi deret: 81 27 9 _ 1?", "id": "3" },
  { "en": "Huruf apakah yang hilang: N O P Q _ S?", "id": "R" },
  { "en": "Bandingkan: (UPPERCASE) vs (uppercase)?", "id": "Beda, huruf kapital." },
  { "en": "Bandingkan: (1,000,000) vs (1000000)?", "id": "Beda, ada koma." },
  { "en": "Bandingkan: (POIUYT) vs (POIUYT)?", "id": "Sama persis." },
  { "en": "Bandingkan: (asdfgh) vs (asdfghj)?", "id": "Beda, kelebihan j." },
  { "en": "Bandingkan: (789789) vs (987987)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Cedera, Cidera, Cedera?", "id": "Cidera" },
  { "en": "Ejaan yang salah: Imbau, Himbau, Imbau, Imbau?", "id": "Himbau" },
  { "en": "Ejaan yang salah: Kompleks, Komplek, Kompleks?", "id": "Komplek" },
  { "en": "Ejaan yang salah: Miliar, Milyar, Miliar?", "id": "Milyar" },
  { "en": "Ejaan yang salah: Teoretis, Teoritis, Teoretis?", "id": "Teoritis" },
  { "en": "Hitung huruf 'm': 'implementasi program'?", "id": "3" },
  { "en": "Hitung angka '1': '111222111333'?", "id": "6" },
  { "en": "Hitung huruf 's': 'asosiasi perusahaan'?", "id": "4" },
  { "en": "Hitung angka '9': '998899779966'?", "id": "6" },
  { "en": "Hitung huruf 'k': 'karakteristik produk'?", "id": "3" },
  { "en": "Mana yang berbeda: LLLLLLLLlLLLLLL?", "id": "l" },
  { "en": "Mana yang aneh: 666666B666666?", "id": "B" },
  { "en": "Simbol mana yang ganjil: :::::::;:::::?", "id": ";" },
  { "en": "Huruf mana yang keliru: TTTTTTTTtTTTTTT?", "id": "t" },
  { "en": "Angka mana yang salah: 111111I111111?", "id": "I" },
  { "en": "Cari yang hilang: (qwertyuiop) vs (qwerty_iop)?", "id": "u" },
  { "en": "Cari yang hilang: (1491625) vs (149_625)?", "id": "1" },
  { "en": "Pola selanjutnya: A D G J M _?", "id": "P" },
  { "en": "Pola selanjutnya: 1 2 4 7 _ 16?", "id": "11" },
  { "en": "Huruf apa yang kosong: J I H G _ E?", "id": "F" },
  { "en": "Lihat pasangan: (xyzabc) vs (xyz abc)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (B8B8B8) vs (8B8B8B)?", "id": "Beda urutan." },
  { "en": "Lihat pasangan: (lowercase) vs (LOWERCASE)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (123456789) vs (123456789)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (mnmnmn) vs (nmnmnm)?", "id": "Beda urutan." },
  { "en": "Tulisan salah: Respons, Respon, Respons?", "id": "Respon" },
  { "en": "Tulisan salah: Saksama, Seksama, Saksama?", "id": "Seksama" },
  { "en": "Tulisan salah: Sekretaris, Sekertaris, Sekretaris?", "id": "Sekertaris" },
  { "en": "Tulisan salah: Silakan, Silahkan, Silakan?", "id": "Silahkan" },
  { "en": "Tulisan salah: Telanjur, Terlanjur, Telanjur?", "id": "Terlanjur" },
  { "en": "Jumlah huruf 'a': 'evaluasi jabatan'?", "id": "4" },
  { "en": "Jumlah angka '3': '3132333435'?", "id": "5" },
  { "en": "Jumlah huruf 'u': 'kualifikasi umum'?", "id": "4" },
  { "en": "Jumlah angka '5': '555444555333'?", "id": "6" },
  { "en": "Jumlah huruf 'b': 'distribusi barang'?", "id": "3" },
  { "en": "Mana yang lain: XXXXXXXxXXXXXXX?", "id": "x" },
  { "en": "Mana yang beda: 555555S555555?", "id": "S" },
  { "en": "Simbol mana yang aneh: <<<<<<<><<<<<?", "id": ">" },
  { "en": "Huruf mana yang salah: UUUUUUUuUUUUUUU?", "id": "u" },
  { "en": "Angka mana yang ganjil: 444444b444444?", "id": "b" },
  { "en": "Hilang satu: (poiuytrewq) vs (poiuyt_ewq)?", "id": "r" },
  { "en": "Hilang satu: (554433) vs (554_33)?", "id": "4" },
  { "en": "Lanjutkan pola: B E H K _ Q?", "id": "N" },
  { "en": "Lanjutkan pola: 5 10 20 40 _ 160?", "id": "80" },
  { "en": "Huruf yang kosong: Q P O N _ L?", "id": "M" },
  { "en": "Periksa pasangan: (test-alpha) vs (testalpha)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (S5S5S5) vs (5S5S5S)?", "id": "Beda urutan." },
  { "en": "Periksa pasangan: (PASSWORD) vs (password)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (246810) vs (246810)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (info@) vs (info)?", "id": "Beda, ada simbol." },
  { "en": "Ejaan keliru: Utang, Hutang, Utang?", "id": "Hutang" },
  { "en": "Ejaan keliru: Varietas, Varitas, Varietas?", "id": "Varitas" },
  { "en": "Ejaan keliru: Verifikasi, Veripikasi, Verifikasi?", "id": "Veripikasi" },
  { "en": "Ejaan keliru: Vila, Villa, Vila?", "id": "Villa" },
  { "en": "Ejaan keliru: Yogyakarta, Jogjakarta, Yogyakarta?", "id": "Jogjakarta" },
  { "en": "Cari yang beda: fffffffffFfffff?", "id": "F" },
  { "en": "Mana yang aneh: 4444444S44444?", "id": "S" },
  { "en": "Tunjukkan huruf ganjil: tttttttttTttttt?", "id": "T" },
  { "en": "Temukan simbol beda: \\\\\\\\\\\\\\|\\\\\\\\\\?", "id": "|" },
  { "en": "Huruf manakah yang salah: zzzzzzzzzZzzzzz?", "id": "Z" },
  { "en": "Angka yang tidak cocok: 999999G999999?", "id": "G" },
  { "en": "Simbol yang berbeda: >>>>>>>>>><<<<<<<<<?", "id": "<" },
  { "en": "Huruf yang tidak sama: jjjjjjjjJjjjjjj?", "id": "J" },
  { "en": "Angka mana yang beda: 22222Z22222?", "id": "Z" },
  { "en": "Simbol mana yang lain: {}{}{}{}[}{}{}{}?", "id": "[" },
  { "en": "Lengkapi: (ghjkl) - (gh_kl)?", "id": "j" },
  { "en": "Lengkapi: (98765) - (98_65)?", "id": "7" },
  { "en": "Lanjutkan pola: Z W T Q _ H?", "id": "N" },
  { "en": "Lanjutkan pola: 10 9 11 8 12 _?", "id": "7" },
  { "en": "Apa huruf yang hilang: P O N M _ K?", "id": "L" },
  { "en": "Sama atau beda: (5S5S5S5) vs (SSSSSSS)?", "id": "Beda, ada 5." },
  { "en": "Sama atau beda: (LKJHGF) vs (LKHJGF)?", "id": "Beda urutan." },
  { "en": "Sama atau beda: (6543210) vs (6543210)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (Kata-Kata) vs (KataKata)?", "id": "Beda, tanpa tanda hubung." },
  { "en": "Sama atau beda: (B8B8B8B) vs (8B8B8B8)?", "id": "Beda urutan." },
  { "en": "Kata yang keliru: Atmosfer, Atmosfir, Atmosfer?", "id": "Atmosfir" },
  { "en": "Kata yang keliru: Baterai, Batere, Baterai?", "id": "Batere" },
  { "en": "Kata yang keliru: Besok, Esok, Besok, Besok?", "id": "Esok" },
  { "en": "Kata yang keliru: Detail, Detil, Detail?", "id": "Detil" },
  { "en": "Kata yang keliru: Elite, Elit, Elite, Elite?", "id": "Elit" },
  { "en": "Berapa huruf 'r': 'prosedur rekrutmen'?", "id": "4" },
  { "en": "Berapa angka '7': '771177227733'?", "id": "6" },
  { "en": "Berapa huruf 'k': 'klasifikasi karakter'?", "id": "4" },
  { "en": "Berapa angka '2': '222444222555'?", "id": "6" },
  { "en": "Berapa huruf 'b': 'subsidi bahan bakar'?", "id": "3" },
  { "en": "Tunjukkan yang beda: VVVVVVVVVvVVVVV?", "id": "v" },
  { "en": "Tunjukkan yang ganjil: 111111l111111?", "id": "l" },
  { "en": "Mana simbol yang lain: ,,,,,,,,,,;,,,,,?", "id": ";" },
  { "en": "Mana huruf yang beda: EEEEEEEEeEEEEEE?", "id": "e" },
  { "en": "Mana angka yang salah: 555555S555555?", "id": "S" },
  { "en": "Pasangan hilang: (poiuyt) vs (poi_yt)?", "id": "u" },
  { "en": "Pasangan hilang: (654321) vs (654_21)?", "id": "3" },
  { "en": "Lengkapi deret: Y V S P _ J?", "id": "M" },
  { "en": "Lengkapi deret: 2 3 5 8 12 _?", "id": "17" },
  { "en": "Huruf apakah yang hilang: G I K M _ Q?", "id": "O" },
  { "en": "Bandingkan: (UPPER-CASE) vs (uppercase)?", "id": "Beda, kapital dan tanda hubung." },
  { "en": "Bandingkan: (5,555) vs (5555)?", "id": "Beda, ada koma." },
  { "en": "Bandingkan: (QAZWSX) vs (QAZWSX)?", "id": "Sama persis." },
  { "en": "Bandingkan: (qwerty) vs (ytrewq)?", "id": "Beda urutan." },
  { "en": "Bandingkan: (123123) vs (321321)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Efektif, Efektip, Efektif?", "id": "Efektip" },
  { "en": "Ejaan yang salah: Fondasi, Pondasi, Fondasi?", "id": "Pondasi" },
  { "en": "Ejaan yang salah: Formal, Formil, Formal?", "id": "Formil" },
  { "en": "Ejaan yang salah: Gizi, Gisi, Gizi, Gizi?", "id": "Gisi" },
  { "en": "Ejaan yang salah: Hafal, Hapal, Hafal, Hafal?", "id": "Hapal" },
  { "en": "Hitung huruf 'n': 'manajemen keuangan'?", "id": "4" },
  { "en": "Hitung angka '8': '818283848'?", "id": "5" },
  { "en": "Hitung huruf 'g': 'regulasi perbankan'?", "id": "2" },
  { "en": "Hitung angka '7': '777666777555'?", "id": "6" },
  { "en": "Hitung huruf 'j': 'proyek jangka panjang'?", "id": "2" },
  { "en": "Mana yang berbeda: NNNNNNNNnNNNNNN?", "id": "n" },
  { "en": "Mana yang aneh: 888888g888888?", "id": "g" },
  { "en": "Simbol mana yang ganjil: :::::::':::::?", "id": "'" },
  { "en": "Huruf mana yang keliru: YYYYYYYyYYYYYYY?", "id": "y" },
  { "en": "Angka mana yang salah: 222222Z222222?", "id": "Z" },
  { "en": "Cari yang hilang: (mnbvcx) vs (mn_vcx)?", "id": "b" },
  { "en": "Cari yang hilang: (13579) vs (135_9)?", "id": "7" },
  { "en": "Pola selanjutnya: 1 4 9 16 _ 36?", "id": "25" },
  { "en": "Pola selanjutnya: Q P O N _ L?", "id": "M" },
  { "en": "Huruf apa yang kosong: A B D E F G?", "id": "C" },
  { "en": "Lihat pasangan: (asdfg) vs (a s d f g)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (IlIlIl) vs (l1l1l1)?", "id": "Beda semua." },
  { "en": "Lihat pasangan: (Kapital) vs (kapital)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (234567) vs (234567)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (ababab) vs (bababa)?", "id": "Beda urutan." },
  { "en": "Tulisan salah: Ikhlas, Ihlas, Ikhlas?", "id": "Ihlas" },
  { "en": "Tulisan salah: Intelijen, Intelejen, Intelijen?", "id": "Intelejen" },
  { "en": "Tulisan salah: Interogasi, Introgasi, Interogasi?", "id": "Introgasi" },
  { "en": "Tulisan salah: Jadwal, Jadual, Jadwal?", "id": "Jadual" },
  { "en": "Tulisan salah: Kategori, Katagori, Kategori?", "id": "Katagori" },
  { "en": "Jumlah huruf 's': 'sistematis dan efisien'?", "id": "4" },
  { "en": "Jumlah angka '6': '6163656769'?", "id": "5" },
  { "en": "Jumlah huruf 'i': 'divisi investigasi'?", "id": "5" },
  { "en": "Jumlah angka '3': '333222333111'?", "id": "6" },
  { "en": "Jumlah huruf 'm': 'museum manajemen modern'?", "id": "5" },
  { "en": "Mana yang lain: OOOOOOO0OOOOOOO?", "id": "0" },
  { "en": "Mana yang beda: 999999P999999?", "id": "P" },
  { "en": "Simbol mana yang aneh: ?????????!?????", "id": "!" },
  { "en": "Huruf mana yang salah: FFFFFFFFfFFFFFF?", "id": "f" },
  { "en": "Angka mana yang ganjil: 888888g888888?", "id": "g" },
  { "en": "Hilang satu: (poiuytrewq) vs (po_uytrewq)?", "id": "i" },
  { "en": "Hilang satu: (121212) vs (121_12)?", "id": "2" },
  { "en": "Lanjutkan pola: C D E F _ H?", "id": "G" },
  { "en": "Lanjutkan pola: 50 45 40 _ 30?", "id": "35" },
  { "en": "Huruf yang kosong: D F H J _ N?", "id": "L" },
  { "en": "Periksa pasangan: (user-name) vs (username)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (G6G6G6) vs (6G6G6G)?", "id": "Beda urutan." },
  { "en": "Periksa pasangan: (USERNAME) vs (username)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (192837) vs (192837)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (web.com) vs (web com)?", "id": "Beda, titik dan spasi." },
  { "en": "Ejaan keliru: Kuitansi, Kwitansi, Kuitansi?", "id": "Kwitansi" },
  { "en": "Ejaan keliru: Lembap, Lembab, Lembap?", "id": "Lembab" },
  { "en": "Ejaan keliru: Lubang, Lobang, Lubang?", "id": "Lobang" },
  { "en": "Ejaan keliru: Maaf, Ma'af, Maaf?", "id": "Ma'af" },
  { "en": "Ejaan keliru: Mahluk, Makhluk, Mahluk?", "id": "Mahluk" },
  { "en": "Cari yang beda: BBBBBBBBBbBBBBB?", "id": "b" },
  { "en": "Mana yang aneh: 6666666G66666?", "id": "G" },
  { "en": "Tunjukkan huruf ganjil: yyyyyyyyyYyyyyy?", "id": "Y" },
  { "en": "Temukan simbol beda: +++++++++++++-+++?", "id": "-" },
  { "en": "Huruf manakah yang salah: eeeeeeeeeEeeeee?", "id": "E" },
  { "en": "Angka yang tidak cocok: 111111I111111?", "id": "I" },
  { "en": "Simbol yang berbeda: ***********+*****?", "id": "+" },
  { "en": "Huruf yang tidak sama: dddddddddDddddd?", "id": "D" },
  { "en": "Angka mana yang beda: 77777T77777?", "id": "T" },
  { "en": "Simbol mana yang lain: =========-=====?", "id": "-" },
  { "en": "Lengkapi: (zxcvbn) - (zxc_bn)?", "id": "v" },
  { "en": "Lengkapi: (192837) - (19_837)?", "id": "2" },
  { "en": "Lanjutkan pola: A B C D E _ G?", "id": "F" },
  { "en": "Lanjutkan pola: 20 18 16 14 _ 10?", "id": "12" },
  { "en": "Apa huruf yang hilang: O P Q _ S T?", "id": "R" },
  { "en": "Sama atau beda: (6G6G6G6) vs (GGGGGGG)?", "id": "Beda, ada 6." },
  { "en": "Sama atau beda: (ZYXWVU) vs (ZYXWVUT)?", "id": "Beda, kelebihan T." },
  { "en": "Sama atau beda: (147258) vs (147258)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (Selamat-Malam) vs (SelamatMalam)?", "id": "Beda, ada tanda hubung." },
  { "en": "Sama atau beda: (S5S5S5S) vs (5S5S5S5)?", "id": "Beda urutan." },
  { "en": "Kata yang keliru: Manajemen, Managemen, Manajemen?", "id": "Managemen" },
  { "en": "Kata yang keliru: Metode, Metoda, Metode?", "id": "Metoda" },
  { "en": "Kata yang keliru: Merek, Merk, Merek, Merek?", "id": "Merk" },
  { "en": "Kata yang keliru: Museum, Musium, Museum?", "id": "Musium" },
  { "en": "Kata yang keliru: Objektif, Obyektif, Objektif?", "id": "Obyektif" },
  { "en": "Berapa huruf 'i': 'distribusi logistik'?", "id": "4" },
  { "en": "Berapa angka '9': '991199229933'?", "id": "6" },
  { "en": "Berapa huruf 'o': 'koordinasi operasional'?", "id": "6" },
  { "en": "Berapa angka '5': '555666555777'?", "id": "6" },
  { "en": "Berapa huruf 'a': 'kapasitas maksimal'?", "id": "6" },
  { "en": "Tunjukkan yang beda: AAAAAAAAAaAAAAA?", "id": "a" },
  { "en": "Tunjukkan yang ganjil: 222222Z222222?", "id": "Z" },
  { "en": "Mana simbol yang lain: '''''''''''\"'''''?", "id": "\"" },
  { "en": "Mana huruf yang beda: TTTTTTTTtTTTTTT?", "id": "t" },
  { "en": "Mana angka yang salah: 999999g999999?", "id": "g" },
  { "en": "Pasangan hilang: (asdfgh) vs (asd_gh)?", "id": "f" },
  { "en": "Pasangan hilang: (36912) vs (36_12)?", "id": "9" },
  { "en": "Lengkapi deret: C G K O _ W?", "id": "S" },
  { "en": "Lengkapi deret: 10 11 13 16 _ 25?", "id": "20" },
  { "en": "Huruf apakah yang hilang: T S R Q _ O?", "id": "P" },
  { "en": "Bandingkan: (FINAL-EXAM) vs (finalexam)?", "id": "Beda, kapital dan tanda hubung." },
  { "en": "Bandingkan: (9.999) vs (9999)?", "id": "Beda, ada titik." },
  { "en": "Bandingkan: (EDCRFV) vs (EDCRFV)?", "id": "Sama persis." },
  { "en": "Bandingkan: (zxcvbn) vs (nbvcxz)?", "id": "Beda urutan." },
  { "en": "Bandingkan: (456456) vs (654654)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Omzet, Omset, Omzet?", "id": "Omset" },
  { "en": "Ejaan yang salah: Orisinal, Orisinil, Orisinal?", "id": "Orisinil" },
  { "en": "Ejaan yang salah: Paham, Faham, Paham?", "id": "Faham" },
  { "en": "Ejaan yang salah: Paradoks, Paradok, Paradoks?", "id": "Paradok" },
  { "en": "Ejaan yang salah: Pasif, Pasip, Pasif, Pasif?", "id": "Pasip" },
  { "en": "Hitung huruf 'p': 'partisipasi publik'?", "id": "3" },
  { "en": "Hitung angka '2': '2123242'?", "id": "4" },
  { "en": "Hitung huruf 'u': 'regulasi perbankan'?", "id": "2" },
  { "en": "Hitung angka '1': '111888111999'?", "id": "6" },
  { "en": "Hitung huruf 'l': 'legalitas formal'?", "id": "4" },
  { "en": "Mana yang berbeda: QQQQQQQQqQQQQQQ?", "id": "q" },
  { "en": "Mana yang aneh: 444444d444444?", "id": "d" },
  { "en": "Simbol mana yang ganjil: ;;;;;;;'';;;;;?", "id": "'" },
  { "en": "Huruf mana yang keliru: ZZZZZZZzZZZZZZZ?", "id": "z" },
  { "en": "Angka mana yang salah: 333333E333333?", "id": "E" },
  { "en": "Cari yang hilang: (poiuyt) vs (poi_yt)?", "id": "u" },
  { "en": "Cari yang hilang: (98765) vs (98_65)?", "id": "7" },
  { "en": "Pola selanjutnya: 1 2 3 5 8 _?", "id": "13" },
  { "en": "Pola selanjutnya: O P Q R _ T?", "id": "S" },
  { "en": "Huruf apa yang kosong: A C F J _ U?", "id": "O" },
  { "en": "Lihat pasangan: (zxcvb) vs (z x c v b)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (g6g6g6) vs (6g6g6g)?", "id": "Beda urutan." },
  { "en": "Lihat pasangan: (Test) vs (test)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (789123) vs (789123)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (cdcdcd) vs (dcdcdc)?", "id": "Beda urutan." },
  { "en": "Tulisan salah: Personel, Personil, Personel?", "id": "Personil" },
  { "en": "Tulisan salah: Pikir, Fikir, Pikir?", "id": "Fikir" },
  { "en": "Tulisan salah: Praktik, Praktek, Praktik?", "id": "Praktek" },
  { "en": "Tulisan salah: Proyek, Projek, Proyek?", "id": "Projek" },
  { "en": "Tulisan salah: Rapi, Rapih, Rapi, Rapi?", "id": "Rapih" },
  { "en": "Jumlah huruf 'a': 'analisis data'?", "id": "5" },
  { "en": "Jumlah angka '4': '4143454749'?", "id": "5" },
  { "en": "Jumlah huruf 'e': 'presentasi efektif'?", "id": "4" },
  { "en": "Jumlah angka '0': '000555000666'?", "id": "6" },
  { "en": "Jumlah huruf 's': 'solusi strategis'?", "id": "4" },
  { "en": "Mana yang lain: WWWWWWWwWWWWWWW?", "id": "w" },
  { "en": "Mana yang beda: 777777S777777?", "id": "S" },
  { "en": "Simbol mana yang aneh: ~~~~~~~~~~`~~~~~?", "id": "`" },
  { "en": "Huruf mana yang salah: AAAAAAAaAAAAAAA?", "id": "a" },
  { "en": "Angka mana yang ganjil: 555555S555555?", "id": "S" },
  { "en": "Hilang satu: (lkjhgf) vs (lkj_gf)?", "id": "h" },
  { "en": "Hilang satu: (336699) vs (336_99)?", "id": "6" },
  { "en": "Lanjutkan pola: D E F G _ I?", "id": "H" },
  { "en": "Lanjutkan pola: 80 75 70 _ 60?", "id": "65" },
  { "en": "Huruf yang kosong: F H J L _ P?", "id": "N" },
  { "en": "Periksa pasangan: (web-site) vs (website)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (H8H8H8) vs (8H8H8H)?", "id": "Beda urutan." },
  { "en": "Periksa pasangan: (WEBSITE) vs (website)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (384756) vs (384756)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (no.1) vs (no 1)?", "id": "Beda, titik dan spasi." },
  { "en": "Ejaan keliru: Realitas, Realita, Realitas?", "id": "Realita" },
  { "en": "Ejaan keliru: Ritel, Retail, Ritel?", "id": "Retail" },
  { "en": "Ejaan keliru: Ritsleting, Resleting, Ritsleting?", "id": "Resleting" },
  { "en": "Ejaan keliru: Roboh, Rubuh, Roboh?", "id": "Rubuh" },
  { "en": "Ejaan keliru: Sekadar, Sekedar, Sekadar?", "id": "Sekedar" },
  { "en": "Cari yang beda: hhhhhhhhhHhhhhh?", "id": "H" },
  { "en": "Mana yang aneh: 5555555G55555?", "id": "G" },
  { "en": "Tunjukkan huruf ganjil: uuuuuuuuuUuuuuu?", "id": "U" },
  { "en": "Temukan simbol beda: &&&&&&&&&%&&&?", "id": "%" },
  { "en": "Huruf manakah yang salah: rrrrrrrrrRrrrrr?", "id": "R" },
  { "en": "Angka yang tidak cocok: 222222Z222222?", "id": "Z" },
  { "en": "Simbol yang berbeda: #########@#####?", "id": "@" },
  { "en": "Huruf yang tidak sama: kkkkkkkkkKkkkkk?", "id": "K" },
  { "en": "Angka mana yang beda: 11111l11111?", "id": "l" },
  { "en": "Simbol mana yang lain: $$$$$$$$$S$$$$$?", "id": "S" },
  { "en": "Lengkapi: (qwerty) - (qwe_ty)?", "id": "r" },
  { "en": "Lengkapi: (13579) - (13_79)?", "id": "5" },
  { "en": "Lanjutkan pola: Z Y X W _ U?", "id": "V" },
  { "en": "Lanjutkan pola: 3 7 11 15 _ 23?", "id": "19" },
  { "en": "Apa huruf yang hilang: M N O _ Q R?", "id": "P" },
  { "en": "Sama atau beda: (8B8B8B8) vs (BBBBBBB)?", "id": "Beda, ada 8." },
  { "en": "Sama atau beda: (PQRSTU) vs (PQRTSU)?", "id": "Beda urutan." },
  { "en": "Sama atau beda: (543210) vs (543210)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (DataAnalyst) vs (Data Analyst)?", "id": "Beda, tanpa spasi." },
  { "en": "Sama atau beda: (I1I1I1I) vs (1I1I1I1)?", "id": "Beda urutan." },
  { "en": "Kata yang keliru: Sintesis, Sintesa, Sintesis?", "id": "Sintesa" },
  { "en": "Kata yang keliru: Sopir, Supir, Sopir, Sopir?", "id": "Supir" },
  { "en": "Kata yang keliru: Terampil, Trampil, Terampil?", "id": "Trampil" },
  { "en": "Kata yang keliru: Tim, Team, Tim, Tim?", "id": "Team" },
  { "en": "Kata yang keliru: Ubah, Rubah, Ubah?", "id": "Rubah" },
  { "en": "Berapa huruf 'k': 'komunikasi publik'?", "id": "3" },
  { "en": "Berapa angka '8': '882288338844'?", "id": "6" },
  { "en": "Berapa huruf 'r': 'infrastruktur negara'?", "id": "4" },
  { "en": "Berapa angka '3': '333444333555'?", "id": "6" },
  { "en": "Berapa huruf 's': 'spesifikasi teknis'?", "id": "4" },
  { "en": "Tunjukkan yang beda: CCCCCCCCCcCCCCC?", "id": "c" },
  { "en": "Tunjukkan yang ganjil: 999999g999999?", "id": "g" },
  { "en": "Mana simbol yang lain: '''''''''''`'''''?", "id": "`" },
  { "en": "Mana huruf yang beda: XXXXXXXXxXXXXXX?", "id": "x" },
  { "en": "Mana angka yang salah: 666666G666666?", "id": "G" },
  { "en": "Pasangan hilang: (zxcvbn) vs (zx_vbn)?", "id": "c" },
  { "en": "Pasangan hilang: (246810) vs (246_10)?", "id": "8" },
  { "en": "Lengkapi deret: M O Q S _ W?", "id": "U" },
  { "en": "Lengkapi deret: 1 1 2 3 5 _?", "id": "8" },
  { "en": "Huruf apakah yang hilang: V U T S _ Q?", "id": "R" },
  { "en": "Bandingkan: (FINAL_SCORE) vs (finalscore)?", "id": "Beda, kapital dan tanda hubung." },
  { "en": "Bandingkan: (8.888) vs (8888)?", "id": "Beda, ada titik." },
  { "en": "Bandingkan: (TGHYUJ) vs (TGHYUJ)?", "id": "Sama persis." },
  { "en": "Bandingkan: (poiuyt) vs (tuyiop)?", "id": "Beda urutan." },
  { "en": "Bandingkan: (789789) vs (987987)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Aktual, Aktuil, Aktual?", "id": "Aktuil" },
  { "en": "Ejaan yang salah: Ambeien, Ambeyen, Ambeien?", "id": "Ambeyen" },
  { "en": "Ejaan yang salah: Astronaut, Astronot, Astronaut?", "id": "Astronot" },
  { "en": "Ejaan yang salah: Blanko, Blangko, Blanko?", "id": "Blangko" },
  { "en": "Ejaan yang salah: Cabai, Cabe, Cabai, Cabai?", "id": "Cabe" },
  { "en": "Hitung huruf 'm': 'manajemen sumber daya'?", "id": "3" },
  { "en": "Hitung angka '9': '919395979'?", "id": "5" },
  { "en": "Hitung huruf 't': 'standarisasi mutu'?", "id": "3" },
  { "en": "Hitung angka '2': '222111222999'?", "id": "6" },
  { "en": "Hitung huruf 'l': 'laporan bulanan'?", "id": "3" },
  { "en": "Mana yang berbeda: PPPPPPPPPpPPPPPPP?", "id": "p" },
  { "en": "Mana yang aneh: 111111I111111?", "id": "I" },
  { "en": "Simbol mana yang ganjil: $$$$$$$;$$$$$$?", "id": ";" },
  { "en": "Huruf mana yang keliru: GGGGGGGgGGGGGGG?", "id": "g" },
  { "en": "Angka mana yang salah: 777777T777777?", "id": "T" },
  { "en": "Cari yang hilang: (asdfghj) vs (asdf_hj)?", "id": "g" },
  { "en": "Cari yang hilang: (147258) vs (147_58)?", "id": "2" },
  { "en": "Pola selanjutnya: 1 5 9 13 _ 21?", "id": "17" },
  { "en": "Pola selanjutnya: U T S R _ P?", "id": "Q" },
  { "en": "Huruf apa yang kosong: C E G I _ M?", "id": "K" },
  { "en": "Lihat pasangan: (lkjhg) vs (l k j h g)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (d4d4d4) vs (4d4d4d)?", "id": "Beda urutan." },
  { "en": "Lihat pasangan: (Report) vs (report)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (369258) vs (369258)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (efefef) vs (fefefe)?", "id": "Beda urutan." },
  { "en": "Tulisan salah: Cendekiawan, Cendikiawan, Cendekiawan?", "id": "Cendikiawan" },
  { "en": "Tulisan salah: Dahulu, Dulu, Dahulu?", "id": "Dulu" },
  { "en": "Tulisan salah: Definisi, Difinisi, Definisi?", "id": "Difinisi" },
  { "en": "Tulisan salah: Depot, Depo, Depot?", "id": "Depo" },
  { "en": "Tulisan salah: Desain, Disain, Desain?", "id": "Disain" },
  { "en": "Jumlah huruf 's': 'sertifikasi profesi'?", "id": "4" },
  { "en": "Jumlah angka '5': '5152535455'?", "id": "6" },
  { "en": "Jumlah huruf 'e': 'departemen keuangan'?", "id": "4" },
  { "en": "Jumlah angka '1': '111000111222'?", "id": "6" },
  { "en": "Jumlah huruf 'b': 'subsidi operasional'?", "id": "2" },
  { "en": "Mana yang lain: YYYYYYYyYYYYYYY?", "id": "y" },
  { "en": "Mana yang beda: 333333E333333?", "id": "E" },
  { "en": "Simbol mana yang aneh: `````````'`````?", "id": "'" },
  { "en": "Huruf mana yang salah: BBBBBBBbBBBBBBB?", "id": "b" },
  { "en": "Angka mana yang ganjil: 888888B888888?", "id": "B" },
  { "en": "Hilang satu: (mnbvcxz) vs (mnbvc_z)?", "id": "x" },
  { "en": "Hilang satu: (112358) vs (1123_8)?", "id": "5" },
  { "en": "Lanjutkan pola: E F G H _ J?", "id": "I" },
  { "en": "Lanjutkan pola: 90 85 80 _ 70?", "id": "75" },
  { "en": "Huruf yang kosong: G J M P _ V?", "id": "S" },
  { "en": "Periksa pasangan: (pass-word) vs (password)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (T9T9T9) vs (9T9T9T)?", "id": "Beda urutan." },
  { "en": "Periksa pasangan: (ADMIN) vs (admin)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (756483) vs (756483)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (file.doc) vs (file doc)?", "id": "Beda, titik dan spasi." },
  { "en": "Ejaan keliru: Detergen, Deterjen, Detergen?", "id": "Deterjen" },
  { "en": "Ejaan keliru: Ekspor, Export, Ekspor?", "id": "Export" },
  { "en": "Ejaan keliru: Embus, Hembus, Embus?", "id": "Hembus" },
  { "en": "Ejaan keliru: Favorit, Pavorit, Favorit?", "id": "Pavorit" },
  { "en": "Ejaan keliru: Fondasi, Pondasi, Fondasi?", "id": "Pondasi" },
  { "en": "Cari yang beda: TTTTTTTTTtTTTTT?", "id": "t" },
  { "en": "Mana yang aneh: 1111111l11111?", "id": "l" },
  { "en": "Tunjukkan huruf ganjil: OOOOOOOOO0OOOOO?", "id": "0" },
  { "en": "Temukan simbol beda: ))))))))))(១១១១១?", "id": "(" },
  { "en": "Huruf manakah yang salah: SSSSSSSSS5SSSSS?", "id": "5" },
  { "en": "Angka yang tidak cocok: 888888B888888?", "id": "B" },
  { "en": "Simbol yang berbeda: /////////\\\\\\?", "id": "\\" },
  { "en": "Huruf yang tidak sama: xxxxxxxxxXxxxxx?", "id": "X" },
  { "en": "Angka mana yang beda: 66666g66666?", "id": "g" },
  { "en": "Simbol mana yang lain: :::::::::;:::::?", "id": ";" },
  { "en": "Lengkapi: (poiuyt) - (poi_yt)?", "id": "u" },
  { "en": "Lengkapi: (97531) - (97_31)?", "id": "5" },
  { "en": "Lanjutkan pola: B C E H L _?", "id": "Q" },
  { "en": "Lanjutkan pola: 5 6 8 11 _ 20?", "id": "15" },
  { "en": "Apa huruf yang hilang: X W V U _ S?", "id": "T" },
  { "en": "Sama atau beda: (7B7B7B7) vs (BBBBBBB)?", "id": "Beda, ada 7." },
  { "en": "Sama atau beda: (MNBVCX) vs (MNVCBX)?", "id": "Beda urutan." },
  { "en": "Sama atau beda: (456789) vs (456789)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (KodePos) vs (Kode Pos)?", "id": "Beda, tanpa spasi." },
  { "en": "Sama atau beda: (Z2Z2Z2Z) vs (2Z2Z2Z2)?", "id": "Beda urutan." },
  { "en": "Kata yang keliru: Fotokopi, Fotocopy, Fotokopi?", "id": "Fotocopy" },
  { "en": "Kata yang keliru: Geladi, Gladi, Geladi?", "id": "Gladi" },
  { "en": "Kata yang keliru: Gua, Goa, Gua, Gua?", "id": "Goa" },
  { "en": "Kata yang keliru: Hadis, Hadits, Hadis?", "id": "Hadits" },
  { "en": "Kata yang keliru: Ideologi, Idiologi, Ideologi?", "id": "Idiologi" },
  { "en": "Berapa huruf 's': 'institusi finansial'?", "id": "4" },
  { "en": "Berapa angka '3': '334433553366'?", "id": "6" },
  { "en": "Berapa huruf 'n': 'konferensi internasional'?", "id": "5" },
  { "en": "Berapa angka '8': '888999888000'?", "id": "6" },
  { "en": "Berapa huruf 'r': 'sumber daya manusia'?", "id": "3" },
  { "en": "Tunjukkan yang beda: KKKKKKKKKkKKKKK?", "id": "k" },
  { "en": "Tunjukkan yang ganjil: 444444b444444?", "id": "b" },
  { "en": "Mana simbol yang lain: '''''''''''\"'''''?", "id": "\"" },
  { "en": "Mana huruf yang beda: LLLLLLLLlLLLLLL?", "id": "l" },
  { "en": "Mana angka yang salah: 555555S555555?", "id": "S" },
  { "en": "Pasangan hilang: (asdfghj) vs (as_fghj)?", "id": "d" },
  { "en": "Pasangan hilang: (11235) vs (11_35)?", "id": "2" },
  { "en": "Lengkapi deret: A C F J _ U?", "id": "O" },
  { "en": "Lengkapi deret: 99 88 77 _ 55?", "id": "66" },
  { "en": "Huruf apakah yang hilang: C D F G H I?", "id": "E" },
  { "en": "Bandingkan: (USER-INTERFACE) vs (userinterface)?", "id": "Beda, kapital dan tanda hubung." },
  { "en": "Bandingkan: (7.777) vs (7777)?", "id": "Beda, ada titik." },
  { "en": "Bandingkan: (TGBYHN) vs (TGBYHN)?", "id": "Sama persis." },
  { "en": "Bandingkan: (zaxscd) vs (zasxcd)?", "id": "Beda urutan." },
  { "en": "Bandingkan: (135135) vs (531531)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Impor, Import, Impor?", "id": "Import" },
  { "en": "Ejaan yang salah: Indera, Indra, Indera?", "id": "Indra" },
  { "en": "Ejaan yang salah: Input, Inpud, Input?", "id": "Inpud" },
  { "en": "Ejaan yang salah: Istri, Isteri, Istri?", "id": "Isteri" },
  { "en": "Ejaan yang salah: Kaidah, Kaedah, Kaidah?", "id": "Kaedah" },
  { "en": "Hitung huruf 'l': 'legalitas perusahaan'?", "id": "3" },
  { "en": "Hitung angka '2': '221122992288'?", "id": "6" },
  { "en": "Hitung huruf 'k': 'kebijakan fiskal'?", "id": "3" },
  { "en": "Hitung angka '6': '666555666444'?", "id": "6" },
  { "en": "Hitung huruf 'v': 'divisi operasional'?", "id": "2" },
  { "en": "Mana yang berbeda: AAAAAAAAAaAAAAAAA?", "id": "a" },
  { "en": "Mana yang aneh: 888888B888888?", "id": "B" },
  { "en": "Simbol mana yang ganjil: :::::::,:::::::?", "id": "," },
  { "en": "Huruf mana yang keliru: VVVVVVVvVVVVVVV?", "id": "v" },
  { "en": "Angka mana yang salah: 444444A444444?", "id": "A" },
  { "en": "Cari yang hilang: (poiuytrew) vs (po_uytrew)?", "id": "i" },
  { "en": "Cari yang hilang: (234567) vs (234_67)?", "id": "5" },
  { "en": "Pola selanjutnya: 1 3 6 10 _ 21?", "id": "15" },
  { "en": "Pola selanjutnya: L M N O _ P?", "id": "Kesalahan, urutan salah." },
  { "en": "Huruf apa yang kosong: D E G H J K?", "id": "F dan I" },
  { "en": "Lihat pasangan: (mnbvc) vs (m n b v c)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (j7j7j7) vs (7j7j7j)?", "id": "Beda urutan." },
  { "en": "Lihat pasangan: (Matrix) vs (matrix)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (852741) vs (852741)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (ghghgh) vs (hghghg)?", "id": "Beda urutan." },
  { "en": "Tulisan salah: Kanker, Kangker, Kanker?", "id": "Kangker" },
  { "en": "Tulisan salah: Karisma, Kharisma, Karisma?", "id": "Kharisma" },
  { "en": "Tulisan salah: Komersial, Komersil, Komersial?", "id": "Komersil" },
  { "en": "Tulisan salah: Kreativitas, Kreatifitas, Kreativitas?", "id": "Kreatifitas" },
  { "en": "Tulisan salah: Kuantitas, Kwantitas, Kuantitas?", "id": "Kwantitas" },
  { "en": "Jumlah huruf 's': 'analisis strategis'?", "id": "4" },
  { "en": "Jumlah angka '5': '5154575'?", "id": "4" },
  { "en": "Jumlah huruf 't': 'departemen terkait'?", "id": "4" },
  { "en": "Jumlah angka '9': '999888999777'?", "id": "6" },
  { "en": "Jumlah huruf 'r': 'direktur utama'?", "id": "2" },
  { "en": "Mana yang lain: XXXXXXXxXXXXXXX?", "id": "x" },
  { "en": "Mana yang beda: 666666G666666?", "id": "G" },
  { "en": "Simbol mana yang aneh: ````'````?", "id": "'" },
  { "en": "Huruf mana yang salah: NNNNNNNnNNNNNNN?", "id": "n" },
  { "en": "Angka mana yang ganjil: 777777T777777?", "id": "T" },
  { "en": "Hilang satu: (lkjhgfd) vs (lkjh_fd)?", "id": "g" },
  { "en": "Hilang satu: (778899) vs (778_99)?", "id": "8" },
  { "en": "Lanjutkan pola: F G H I _ K?", "id": "J" },
  { "en": "Lanjutkan pola: 70 65 60 _ 50?", "id": "55" },
  { "en": "Huruf yang kosong: H K N Q _ W?", "id": "T" },
  { "en": "Periksa pasangan: (sub-domain) vs (subdomain)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (K1K1K1) vs (1K1K1K)?", "id": "Beda urutan." },
  { "en": "Periksa pasangan: (SYSTEM) vs (system)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (987123) vs (987123)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (file.exe) vs (file exe)?", "id": "Beda, titik dan spasi." },
  { "en": "Ejaan keliru: Legalisasi, Legalisir, Legalisasi?", "id": "Legalisir" },
  { "en": "Ejaan keliru: Massal, Masal, Massal?", "id": "Masal" },
  { "en": "Ejaan keliru: Memesona, Mempesona, Memesona?", "id": "Mempesona" },
  { "en": "Ejaan keliru: Menerapkan, Menterapkan, Menerapkan?", "id": "Menterapkan" },
  { "en": "Ejaan keliru: Merek, Merk, Merek?", "id": "Merk" },
  { "en": "Cari yang beda: GGGGGGGGGgGGGGG?", "id": "g" },
  { "en": "Mana yang aneh: 1111111I11111?", "id": "I" },
  { "en": "Tunjukkan huruf ganjil: BBBBBBBBB8BBBBB?", "id": "8" },
  { "en": "Temukan simbol beda: <<<<<<<<<<<<<<<>>?", "id": ">" },
  { "en": "Huruf manakah yang salah: ZZZZZZZZZzZZZZZ?", "id": "z" },
  { "en": "Angka yang tidak cocok: 444444b444444?", "id": "b" },
  { "en": "Simbol yang berbeda: ~~~~~~~~~~`~~~~~?", "id": "`" },
  { "en": "Huruf yang tidak sama: uuuuuuuuuUuuuuu?", "id": "U" },
  { "en": "Angka mana yang beda: 55555S55555?", "id": "S" },
  { "en": "Simbol mana yang lain: !!!!!!!!!!!!!.?!!", "id": "." },
  { "en": "Lengkapi: (lkjhgf) - (lkj_gf)?", "id": "h" },
  { "en": "Lengkapi: (246810) - (24_810)?", "id": "6" },
  { "en": "Lanjutkan pola: D H L P _ X?", "id": "T" },
  { "en": "Lanjutkan pola: 101 202 303 _ 505?", "id": "404" },
  { "en": "Apa huruf yang hilang: Z Y X _ V U?", "id": "W" },
  { "en": "Sama atau beda: (4d4d4d4) vs (ddddddd)?", "id": "Beda, ada 4." },
  { "en": "Sama atau beda: (QWERTY) vs (YTREWQ)?", "id": "Beda urutan." },
  { "en": "Sama atau beda: (3691215) vs (3691215)?", "id": "Sama persis." },
  { "en": "Sama atau beda: (Laporan_Bulanan) vs (LaporanBulanan)?", "id": "Beda, ada garis bawah." },
  { "en": "Sama atau beda: (A1A1A1A) vs (1A1A1A1)?", "id": "Beda urutan." },
  { "en": "Kata yang keliru: Miliar, Milyar, Miliar?", "id": "Milyar" },
  { "en": "Kata yang keliru: Konsekuen, Konsekwen, Konsekuen?", "id": "Konsekwen" },
  { "en": "Kata yang keliru: Kuesioner, Kuisioner, Kuesioner?", "id": "Kuisioner" },
  { "en": "Kata yang keliru: Memengaruhi, Mempengaruhi, Memengaruhi?", "id": "Mempengaruhi" },
  { "en": "Kata yang keliru: Napas, Nafas, Napas?", "id": "Nafas" },
  { "en": "Berapa huruf 't': 'otoritas tertinggi'?", "id": "5" },
  { "en": "Berapa angka '1': '112211331144'?", "id": "6" },
  { "en": "Berapa huruf 'k': 'karakteristik subjek'?", "id": "3" },
  { "en": "Berapa angka '4': '444555444666'?", "id": "6" },
  { "en": "Berapa huruf 'b': 'distribusi probabilitas'?", "id": "3" },
  { "en": "Tunjukkan yang beda: MMMMMMMMMmMMMMM?", "id": "m" },
  { "en": "Tunjukkan yang ganjil: 777777S777777?", "id": "S" },
  { "en": "Mana simbol yang lain: ?????????!?????", "id": "!" },
  { "en": "Mana huruf yang beda: WWWWWWWwWWWWWWW?", "id": "w" },
  { "en": "Mana angka yang salah: 222222S222222?", "id": "S" },
  { "en": "Pasangan hilang: (poiuytr) vs (poi_ytr)?", "id": "u" },
  { "en": "Pasangan hilang: (963852) vs (963_52)?", "id": "8" },
  { "en": "Lengkapi deret: E H K N _ T?", "id": "Q" },
  { "en": "Lengkapi deret: 3 9 27 _ 243?", "id": "81" },
  { "en": "Huruf apakah yang hilang: R P N L _ H?", "id": "J" },
  { "en": "Bandingkan: (FINAL-VERSION) vs (finalversion)?", "id": "Beda, kapital dan tanda hubung." },
  { "en": "Bandingkan: (6,666) vs (6666)?", "id": "Beda, ada koma." },
  { "en": "Bandingkan: (RFVBGTY) vs (RFVBGTY)?", "id": "Sama persis." },
  { "en": "Bandingkan: (abcdef) vs (fedcba)?", "id": "Beda urutan." },
  { "en": "Bandingkan: (102030) vs (302010)?", "id": "Beda urutan." },
  { "en": "Ejaan yang salah: Netralisasi, Netralisir, Netralisasi?", "id": "Netralisir" },
  { "en": "Ejaan yang salah: Diagnosis, Diagnosa, Diagnosis?", "id": "Diagnosa" },
  { "en": "Ejaan yang salah: Normalisasi, Normalisir, Normalisasi?", "id": "Normalisir" },
  { "en": "Ejaan yang salah: Orisinalitas, Orisinalitas, Orisinilitas?", "id": "Orisinilitas" },
  { "en": "Ejaan yang salah: Paradigma, Paradikma, Paradigma?", "id": "Paradikma" },
  { "en": "Hitung huruf 'n': 'koordinasi internal'?", "id": "4" },
  { "en": "Hitung angka '2': '212426282'?", "id": "5" },
  { "en": "Hitung huruf 'r': 'verifikasi berkas'?", "id": "3" },
  { "en": "Hitung angka '0': '000111000222'?", "id": "6" },
  { "en": "Hitung huruf 'j': 'objek dan subjek'?", "id": "2" },
  { "en": "Mana yang berbeda: NNNNNNNNnNNNNNN?", "id": "n" },
  { "en": "Mana yang aneh: 444444b444444?", "id": "b" },
  { "en": "Simbol mana yang ganjil: ========;=======?", "id": ";" },
  { "en": "Huruf mana yang keliru: SSSSSSSsSSSSSSS?", "id": "s" },
  { "en": "Angka mana yang salah: 111111l111111?", "id": "l" },
  { "en": "Cari yang hilang: (mnbvcxz) vs (mnbv_xz)?", "id": "c" },
  { "en": "Cari yang hilang: (112233) vs (1_2233)?", "id": "1" },
  { "en": "Pola selanjutnya: 1 2 4 8 _ 32?", "id": "16" },
  { "en": "Pola selanjutnya: T S R Q _ P?", "id": "Kesalahan, urutan salah." },
  { "en": "Huruf apa yang kosong: B D G K _ V?", "id": "P" },
  { "en": "Lihat pasangan: (qwerty) vs (q w e r t y)?", "id": "Beda, ada spasi." },
  { "en": "Lihat pasangan: (x5x5x5) vs (5x5x5x)?", "id": "Beda urutan." },
  { "en": "Lihat pasangan: (System) vs (system)?", "id": "Beda, huruf kapital." },
  { "en": "Lihat pasangan: (475869) vs (475869)?", "id": "Sama persis." },
  { "en": "Lihat pasangan: (ererer) vs (rerere)?", "id": "Beda urutan." },
  { "en": "Tulisan salah: Pembaruan, Pembaharuan, Pembaruan?", "id": "Pembaharuan" },
  { "en": "Tulisan salah: Penglihatan, Pelihatan, Penglihatan?", "id": "Pelihatan" },
  { "en": "Tulisan salah: Perajin, Pengrajin, Perajin?", "id": "Pengrajin" },
  { "en": "Tulisan salah: Persentase, Prosentase, Persentase?", "id": "Prosentase" },
  { "en": "Tulisan salah: Ramai, Rame, Ramai, Ramai?", "id": "Rame" },
  { "en": "Jumlah huruf 'a': 'validasi data internal'?", "id": "6" },
  { "en": "Jumlah angka '5': '525456585'?", "id": "5" },
  { "en": "Jumlah huruf 'e': 'implementasi regulasi'?", "id": "4" },
  { "en": "Jumlah angka '8': '888777888666'?", "id": "6" },
  { "en": "Jumlah huruf 'p': 'partisipasi populer'?", "id": "4" },
  { "en": "Mana yang lain: KKKKKKKkKKKKKKK?", "id": "k" },
  { "en": "Mana yang beda: 555555B555555?", "id": "B" },
  { "en": "Simbol mana yang aneh: \"\"\"\"\"\"\"\"\"'\"\"\"\"\"?", "id": "'" },
  { "en": "Huruf mana yang salah: LLLLLLLlLLLLLLL?", "id": "l" },
  { "en": "Angka mana yang ganjil: 999999P999999?", "id": "P" },
  { "en": "Hilang satu: (qazwsxedc) vs (qazws_edc)?", "id": "x" },
  { "en": "Hilang satu: (101001) vs (101_01)?", "id": "0" },
  { "en": "Lanjutkan pola: H I J K _ M?", "id": "L" },
  { "en": "Lanjutkan pola: 60 55 50 _ 40?", "id": "45" },
  { "en": "Huruf yang kosong: I L O R _ X?", "id": "U" },
  { "en": "Periksa pasangan: (read-only) vs (readonly)?", "id": "Beda, ada tanda hubung." },
  { "en": "Periksa pasangan: (L7L7L7) vs (7L7L7L)?", "id": "Beda urutan." },
  { "en": "Periksa pasangan: (SECURITY) vs (security)?", "id": "Beda, huruf kapital." },
  { "en": "Periksa pasangan: (951753) vs (951753)?", "id": "Sama persis." },
  { "en": "Periksa pasangan: (data.json) vs (data json)?", "id": "Beda, titik dan spasi." },
  { "en": "Ejaan keliru: Risiko, Resiko, Risiko?", "id": "Resiko" },
  { "en": "Ejaan keliru: Saksama, Seksama, Saksama?", "id": "Seksama" },
  { "en": "Ejaan keliru: Sambal, Sambel, Sambal?", "id": "Sambel" },
  { "en": "Ejaan keliru: Saraf, Syaraf, Saraf?", "id": "Syaraf" },
  { "en": "Ejaan keliru: Satria, Kesatria, Satria?", "id": "Kesatria" }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
