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


    { "en": "Apa Tujuan Utama Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Linear Yang Kompleks." },
    { "en": "Rangkaian Ekuivalen Thevenin Terdiri Dari Apa?", "id": "Sumber Tegangan Seri Dengan Resistor." },
    { "en": "Apa Nama Sumber Tegangan Ekuivalen Thevenin?", "id": "Tegangan Thevenin (Vth)." },
    { "en": "Apa Nama Resistor Ekuivalen Thevenin?", "id": "Resistansi Thevenin (Rth)." },
    { "en": "Bagaimana Cara Menemukan Tegangan Thevenin?", "id": "Ukur Tegangan Pada Terminal Terbuka." },
    { "en": "Tegangan Thevenin Sama Dengan Tegangan Apa?", "id": "Tegangan Rangkaian Terbuka (Voc)." },
    { "en": "Apa Langkah Pertama Mencari Ekuivalen Thevenin?", "id": "Lepaskan Komponen Beban Dari Rangkaian." },
    { "en": "Bagaimana Cara Menemukan Resistansi Thevenin?", "id": "Lihat Resistansi Dari Terminal Terbuka." },
    { "en": "Apa Yang Dilakukan Pada Sumber Tegangan Independen?", "id": "Diganti Dengan Hubung Singkat." },
    { "en": "Apa Yang Dilakukan Pada Sumber Arus Independen?", "id": "Diganti Dengan Rangkaian Terbuka." },
    { "en": "Apa Tujuan Utama Teorema Norton?", "id": "Juga Untuk Menyederhanakan Rangkaian Linear." },
    { "en": "Rangkaian Ekuivalen Norton Terdiri Dari Apa?", "id": "Sumber Arus Paralel Dengan Resistor." },
    { "en": "Apa Nama Sumber Arus Ekuivalen Norton?", "id": "Arus Norton (In)." },
    { "en": "Apa Nama Resistor Ekuivalen Norton?", "id": "Resistansi Norton (Rn)." },
    { "en": "Bagaimana Cara Menemukan Arus Norton?", "id": "Ukur Arus Pada Terminal Hubung Singkat." },
    { "en": "Arus Norton Sama Dengan Arus Apa?", "id": "Arus Hubung Singkat (Isc)." },
    { "en": "Bagaimana Hubungan Antara Resistansi Norton Dan Thevenin?", "id": "Nilainya Selalu Sama Persis (Rn = Rth)." },
    { "en": "Apa Hubungan Antara Vth, In, Dan Rth?", "id": "Vth Sama Dengan In Dikalikan Rth." },
    { "en": "Prinsip Ini Dikenal Sebagai Transformasi Apa?", "id": "Transformasi Sumber." },
    { "en": "Teorema Ini Berlaku Untuk Jenis Rangkaian Apa?", "id": "Hanya Rangkaian Listrik Linear." },
    { "en": "Terminal Yang Dianalisis Disebut Juga Apa?", "id": "Sepasang Terminal Atau Port." },
    { "en": "Mengapa Teorema Ini Sangat Berguna?", "id": "Memudahkan Analisis Dengan Beban Bervariasi." },
    { "en": "Apakah Sumber Terkendali Dinonaktifkan Saat Mencari Rth?", "id": "Tidak, Sumber Terkendali Tetap Aktif." },
    { "en": "Bagaimana Mencari Rth Jika Ada Sumber Terkendali?", "id": "Menggunakan Sumber Uji Eksternal." },
    { "en": "Sumber Uji Apa Saja Yang Bisa Digunakan?", "id": "Sumber Tegangan Uji Atau Sumber Arus Uji." },
    { "en": "Rth Dihitung Sebagai Rasio Apa Dengan Sumber Uji?", "id": "Rasio Tegangan Uji Terhadap Arus Uji." },
    { "en": "Cara Alternatif Mencari Rth?", "id": "Rth Sama Dengan Voc Dibagi Isc." },
    { "en": "Metode Voc/Isc Berguna Untuk Rangkaian Apa?", "id": "Rangkaian Dengan Sumber Terkendali." },
    { "en": "Apakah Komponen Beban Termasuk Dalam Perhitungan Ekuivalen?", "id": "Tidak, Beban Harus Dilepaskan Terlebih Dahulu." },
    { "en": "Rangkaian Ekuivalen Thevenin Adalah Dual Dari Apa?", "id": "Rangkaian Ekuivalen Norton." },
    { "en": "Siapa Nama Penemu Teorema Thevenin?", "id": "Léon Charles Thévenin." },
    { "en": "Siapa Nama Penemu Teorema Norton?", "id": "Edward Lawry Norton." },
    { "en": "Kapan Analisis Nodal Berguna Dalam Thevenin?", "id": "Untuk Menemukan Tegangan Rangkaian Terbuka (Voc)." },
    { "en": "Kapan Analisis Mesh Berguna Dalam Norton?", "id": "Untuk Menemukan Arus Hubung Singkat (Isc)." },
    { "en": "Setelah Mendapat Rangkaian Ekuivalen, Apa Langkah Selanjutnya?", "id": "Hubungkan Kembali Beban Ke Rangkaian Ekuivalen." },
    { "en": "Bagaimana Menghitung Arus Beban Dengan Rangkaian Thevenin?", "id": "Menggunakan Hukum Ohm Sederhana." },
    { "en": "Rumus Arus Beban Thevenin?", "id": "IL = Vth / (Rth + RL)." },
    { "en": "Bagaimana Menghitung Tegangan Beban Dengan Rangkaian Thevenin?", "id": "Menggunakan Aturan Pembagi Tegangan." },
    { "en": "Rumus Tegangan Beban Thevenin?", "id": "VL = Vth * RL / (Rth + RL)." },
    { "en": "Bagaimana Menghitung Arus Beban Dengan Rangkaian Norton?", "id": "Menggunakan Aturan Pembagi Arus." },
    { "en": "Rumus Arus Beban Norton?", "id": "IL = In * Rn / (Rn + RL)." },
    { "en": "Teorema Ini Menyederhanakan Rangkaian Dari Sudut Pandang Siapa?", "id": "Dari Sudut Pandang Komponen Beban." },
    { "en": "Apakah Rangkaian Ekuivalen Sama Dengan Rangkaian Asli?", "id": "Tidak, Tapi Perilakunya Sama Di Terminal." },
    { "en": "Apa Arti 'Ekuivalen' Dalam Konteks Ini?", "id": "Memiliki Karakteristik V-I Yang Sama." },
    { "en": "Bisakah Vth Bernilai Nol?", "id": "Ya, Jika Rangkaian Adalah Jembatan Seimbang." },
    { "en": "Jika Vth Nol, Rangkaian Thevenin Menjadi Apa?", "id": "Hanya Sebuah Resistor (Rth)." },
    { "en": "Bisakah Rth Bernilai Nol?", "id": "Ya, Jika Rangkaian Internal Adalah Sumber Ideal." },
    { "en": "Jika Rth Nol, Rangkaian Thevenin Menjadi Apa?", "id": "Sumber Tegangan Ideal (Vth)." },
    { "en": "Bisakah Rth Tak Terhingga?", "id": "Ya, Jika Rangkaian Internal Sumber Arus Ideal." },
    { "en": "Jika Rth Tak Terhingga, Rangkaian Norton Menjadi Apa?", "id": "Sumber Arus Ideal (In)." },
    { "en": "Apakah Teorema Ini Berlaku Untuk Rangkaian AC?", "id": "Ya, Dengan Menggunakan Konsep Impedansi." },
    { "en": "Dalam Rangkaian AC, Vth Dan In Menjadi Apa?", "id": "Menjadi Fasor." },
    { "en": "Dalam Rangkaian AC, Rth Dan Rn Menjadi Apa?", "id": "Menjadi Impedansi (Zth Dan Zn)." },
    { "en": "Bagaimana Mencari Impedansi Thevenin (Zth)?", "id": "Sama Seperti Rth, Tapi Untuk Komponen AC." },
    { "en": "Impedansi Kapasitor Diperlakukan Seperti Apa Saat Sumber DC Mati?", "id": "Tidak Ada Pengaruhnya." },
    { "en": "Impedansi Induktor Diperlakukan Seperti Apa Saat Sumber DC Mati?", "id": "Tidak Ada Pengaruhnya." },
    { "en": "Apakah Teorema Ini Bisa Diterapkan Pada Dioda?", "id": "Tidak, Karena Dioda Adalah Komponen Non-Linear." },
    { "en": "Apa Itu 'Look-back Resistance'?", "id": "Nama Lain Untuk Resistansi Thevenin." },
    { "en": "Mengapa Disebut 'Look-back'?", "id": "Karena Kita Melihat Kembali Ke Dalam Rangkaian." },
    { "en": "Apakah Arah Polaritas Vth Penting?", "id": "Sangat Penting Untuk Perhitungan Yang Benar." },
    { "en": "Apakah Arah Arus In Penting?", "id": "Sangat Penting Untuk Perhitungan Yang Benar." },
    { "en": "Teorema Superposisi Bisa Digunakan Untuk Mencari Vth?", "id": "Ya, Dengan Menjumlahkan Tegangan Dari Setiap Sumber." },
    { "en": "Apa Arti Fisik Dari Rth?", "id": "Resistansi Internal Rangkaian Sumber." },
    { "en": "Sumber Tegangan Praktis Dimodelkan Sebagai Apa?", "id": "Sumber Ideal Seri Dengan Resistor Internal." },
    { "en": "Model Ini Adalah Rangkaian Ekuivalen Apa?", "id": "Rangkaian Ekuivalen Thevenin." },
    { "en": "Sumber Arus Praktis Dimodelkan Sebagai Apa?", "id": "Sumber Ideal Paralel Dengan Resistor Internal." },
    { "en": "Model Ini Adalah Rangkaian Ekuivalen Apa?", "id": "Rangkaian Ekuivalen Norton." },
    { "en": "Dapatkah Rth Bernilai Negatif?", "id": "Ya, Jika Ada Sumber Terkendali." },
    { "en": "Resistansi Negatif Berarti Rangkaian Bertindak Sebagai Apa?", "id": "Sebagai Sumber Energi, Bukan Beban." },
    { "en": "Bagaimana Jika Rangkaian Hanya Terdiri Dari Resistor?", "id": "Maka Vth Dan In Adalah Nol." },
    { "en": "Jika Rangkaian Hanya Resistor, Rth Sama Dengan Apa?", "id": "Resistansi Ekuivalen Totalnya." },
    { "en": "Apakah Pilihan Terminal Mempengaruhi Hasil Ekuivalen?", "id": "Ya, Rangkaian Ekuivalen Spesifik Untuk Sepasang Terminal." },
    { "en": "Teorema Ini Termasuk Dalam Analisis Rangkaian Apa?", "id": "Analisis Keadaan Tunak (Steady-State)." },
    { "en": "Kapan Teorema Thevenin Ditemukan?", "id": "Pada Tahun 1883." },
    { "en": "Apakah Thevenin Dan Norton Bekerja Bersama?", "id": "Tidak, Mereka Bekerja Di Laboratorium Berbeda." },
    { "en": "Teorema Norton Dipublikasikan Kapan?", "id": "Pada Tahun 1926." },
    { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Menentukan Beban Untuk Daya Maksimal." },
    { "en": "Kondisi Daya Maksimum Terkait Dengan Teorema Apa?", "id": "Terkait Erat Dengan Teorema Thevenin." },
    { "en": "Kapan Daya Maksimum Ditransfer Ke Beban?", "id": "Ketika Resistansi Beban Sama Dengan Resistansi Thevenin." },
    { "en": "Jadi, Syaratnya Adalah RL = ?", "id": "RL = Rth." },
    { "en": "Berapa Nilai Daya Maksimum Yang Ditransfer?", "id": "Pmax = Vth Kuadrat Dibagi Empat Rth." },
    { "en": "Kondisi Ini Disebut Juga Sebagai Apa?", "id": "Pencocokan Beban (Load Matching)." },
    { "en": "Berapa Efisiensi Saat Daya Maksimum Ditransfer?", "id": "Lima Puluh Persen (50%)." },
    { "en": "Di Mana Aplikasi Transfer Daya Maksimum Penting?", "id": "Dalam Sistem Komunikasi Dan Audio." },
    { "en": "Mengapa Tidak Digunakan Dalam Jaringan Listrik PLN?", "id": "Karena Efisiensi Rendah Tidak Diinginkan." },
    { "en": "Apa Tujuan Utama Jaringan Listrik?", "id": "Transfer Energi Dengan Efisiensi Maksimal." },
    { "en": "Bagaimana Mendapatkan Efisiensi Tinggi?", "id": "Membuat Resistansi Beban Jauh Lebih Besar Dari Rth." },
    { "en": "Bagaimana Kondisi Daya Maksimum Untuk Rangkaian AC?", "id": "Ketika Impedansi Beban Adalah Konjugat Kompleks." },
    { "en": "Jika Zth = R+jX, Maka ZL Harus Apa?", "id": "ZL = R-jX." },
    { "en": "Proses Ini Disebut Pencocokan Apa?", "id": "Pencocokan Konjugat (Conjugate Matching)." },
    { "en": "Apa Tujuan Memilih Reaktansi Beban Yang Berlawanan?", "id": "Untuk Mencapai Kondisi Resonansi Dalam Rangkaian." },
    { "en": "Pada Resonansi, Reaktansi Total Menjadi Apa?", "id": "Menjadi Nol." },
    { "en": "Apakah Analisis Rangkaian Selalu Memiliki Satu Jawaban?", "id": "Ya, Untuk Rangkaian Yang Didefinisikan Dengan Baik." },
    { "en": "Teorema Thevenin Adalah Alat Untuk Apa?", "id": "Alat Analisis Untuk Menyederhanakan Masalah." },
    { "en": "Apakah Hasil Akhir Arus Beban Sama?", "id": "Ya, Apapun Metode Analisis Yang Digunakan." },
    { "en": "Apa Langkah Pertama Dalam Analisis Thevenin?", "id": "Identifikasi Dan Lepaskan Komponen Beban." },
    { "en": "Apa Langkah Kedua Dalam Analisis Thevenin?", "id": "Hitung Tegangan Rangkaian Terbuka (Vth)." },
    { "en": "Apa Langkah Ketiga Dalam Analisis Thevenin?", "id": "Hitung Resistansi Thevenin (Rth)." },
    { "en": "Apa Langkah Terakhir Dalam Analisis Thevenin?", "id": "Gambar Rangkaian Ekuivalen Dan Hubungkan Beban." },
    { "en": "Bagaimana Cara Mematikan Sumber Tegangan?", "id": "Menggantinya Dengan Sebuah Kawat (Hubung Singkat)." },
    { "en": "Bagaimana Cara Mematikan Sumber Arus?", "id": "Dengan Menghapusnya (Rangkaian Terbuka)." },
    { "en": "Menonaktifkan Sumber Juga Disebut Apa?", "id": "Membuat Sumber Menjadi Nol." },
    { "en": "Apa Tegangan Pada Hubung Singkat Ideal?", "id": "Nol Volt." },
    { "en": "Apa Arus Pada Rangkaian Terbuka Ideal?", "id": "Nol Ampere." },
    { "en": "Untuk Mencari Rth, Kita Melihat Dari Mana?", "id": "Dari Terminal Beban Yang Terbuka." },
    { "en": "Apa Itu 'Terminal Beban'?", "id": "Dua Titik Tempat Beban Dihubungkan." },
    { "en": "Bisakah Teorema Thevenin Diterapkan Pada Bagian Manapun?", "id": "Ya, Pada Bagian Linear Manapun." },
    { "en": "Apa Itu Rangkaian Jembatan Wheatstone?", "id": "Rangkaian Empat Resistor Berbentuk Berlian." },
    { "en": "Bagaimana Menganalisis Jembatan Tidak Seimbang?", "id": "Gunakan Thevenin Di Antara Titik Tengah." },
    { "en": "Apa Kondisi Jembatan Seimbang?", "id": "Ketika Perkalian Silang Resistor Sama." },
    { "en": "Berapa Vth Pada Jembatan Seimbang?", "id": "Nol Volt." },
    { "en": "Apa Arus Yang Mengalir Melalui Galvanometer Seimbang?", "id": "Nol Ampere." },
    { "en": "Apa Langkah Pertama Dalam Analisis Norton?", "id": "Lepaskan Komponen Beban." },
    { "en": "Apa Langkah Kedua Dalam Analisis Norton?", "id": "Hitung Arus Hubung Singkat (In)." },
    { "en": "Apa Langkah Ketiga Dalam Analisis Norton?", "id": "Hitung Resistansi Norton (Rn)." },
    { "en": "Bagaimana Cara Membuat Hubung Singkat?", "id": "Hubungkan Terminal Beban Dengan Kawat Ideal." },
    { "en": "Metode Menemukan Rn Sama Dengan Metode Apa?", "id": "Metode Menemukan Rth." },
    { "en": "Apakah Hasil Akhir Arus Beban Akan Sama?", "id": "Ya, Harus Sama Persis." },
    { "en": "Metode Mana Yang Lebih Baik?", "id": "Tergantung Pada Struktur Rangkaian." },
    { "en": "Jika Rangkaian Memiliki Banyak Simpul, Thevenin Mudah?", "id": "Ya, Gunakan Analisis Nodal Untuk Vth." },
    { "en": "Jika Rangkaian Memiliki Banyak Loop, Norton Mudah?", "id": "Ya, Gunakan Analisis Mesh Untuk Isc." },
    { "en": "Apa Itu 'Sumber Terkendali'?", "id": "Sumber Yang Nilainya Tergantung Variabel Lain." },
    { "en": "Contoh Variabel Lain?", "id": "Tegangan Atau Arus Di Bagian Lain." },
    { "en": "Apakah Sumber Terkendali Dimatikan Saat Mencari Rth?", "id": "Tidak, Tidak Boleh Dimatikan." },
    { "en": "Mengapa Sumber Terkendali Tidak Boleh Mati?", "id": "Karena Merupakan Bagian Aktif Dari Rangkaian." },
    { "en": "Apa Itu 'Sumber Uji'?", "id": "Sumber Eksternal Dengan Nilai Yang Diketahui." },
    { "en": "Kapan 'Sumber Uji' Digunakan?", "id": "Untuk Mencari Rth Dengan Sumber Terkendali." },
    { "en": "Nilai Sumber Uji Yang Biasa Dipakai?", "id": "Satu Volt Atau Satu Ampere." },
    { "en": "Mengapa Nilai Satu Dipakai?", "id": "Untuk Menyederhanakan Perhitungan Matematika." },
    { "en": "Jika Sumber Uji 1V Diterapkan, Rth Adalah?", "id": "Rth = 1V / I_uji." },
    { "en": "Jika Sumber Uji 1A Diterapkan, Rth Adalah?", "id": "Rth = V_uji / 1A." },
    { "en": "Saat Memakai Sumber Uji, Sumber Internal Lain Bagaimana?", "id": "Harus Dinonaktifkan (Jika Independen)." },
    { "en": "Apakah Thevenin/Norton Mengubah Rangkaian Asli?", "id": "Tidak, Hanya Membuat Model Ekuivalen." },
    { "en": "Apakah Daya Yang Dihasilkan Rangkaian Ekuivalen Sama?", "id": "Tidak, Daya Internalnya Berbeda." },
    { "en": "Teorema Ini Hanya Akurat Untuk Perilaku Di Mana?", "id": "Di Terminal Eksternal (Beban)." },
    { "en": "Apa Itu 'Black Box'?", "id": "Sistem Yang Internalnya Tidak Diketahui." },
    { "en": "Bisakah Thevenin/Norton Diterapkan Pada 'Black Box'?", "id": "Ya, Dengan Melakukan Pengukuran Eksternal." },
    { "en": "Pengukuran Apa Yang Diperlukan?", "id": "Mengukur Voc Dan Isc." },
    { "en": "Dari Voc Dan Isc, Bagaimana Menemukan Rth?", "id": "Rth = Voc / Isc." },
    { "en": "Metode Pengukuran Ini Sangat Berguna Di Mana?", "id": "Dalam Praktik Laboratorium." },
    { "en": "Apakah Teorema Thevenin/Norton Hanya Untuk Rangkaian DC?", "id": "Tidak, Juga Berlaku Untuk Rangkaian AC." },
    { "en": "Untuk Rangkaian AC, Resistansi Diganti Dengan Apa?", "id": "Impedansi (Z)." },
    { "en": "Tegangan Dan Arus Diganti Dengan Apa?", "id": "Fasor." },
    { "en": "Jadi Zth Adalah Impedansi Thevenin?", "id": "Ya, Benar." },
    { "en": "Bagaimana Cara 'Mematikan' Kapasitor Di Analisis Zth?", "id": "Biarkan Saja Sebagai Impedansinya." },
    { "en": "Sumber AC Dimatikan Menjadi Apa?", "id": "Sama Seperti Sumber DC (Singkat/Terbuka)." },
    { "en": "Teorema Ini Adalah Alat Fundamental Dalam Apa?", "id": "Analisis Rangkaian Listrik." },
    { "en": "Apa Manfaat Terbesar Teorema Ini?", "id": "Mengurangi Kerumitan Rangkaian Secara Drastis." },
    { "en": "Apakah Valid Untuk Rangkaian Dengan Transformator?", "id": "Ya, Jika Transformatornya Dianggap Ideal Dan Linear." },
    { "en": "Apakah Valid Untuk Rangkaian Dengan Op-Amp?", "id": "Ya, Jika Op-Amp Bekerja Di Daerah Linear." },
    { "en": "Bagaimana Jika Beban Adalah Sumber Tegangan?", "id": "Teorema Masih Berlaku." },
    { "en": "Bagaimana Polaritas Vth Ditentukan?", "id": "Berdasarkan Beda Potensial Antara Terminal A Dan B." },
    { "en": "Bagaimana Arah In Ditentukan?", "id": "Arus Yang Mengalir Dari Terminal A Ke B." },
    { "en": "Apakah Mungkin Mendapat Hasil Vth Atau In Negatif?", "id": "Ya, Tergantung Pada Polaritas/Arah Referensi." },
    { "en": "Nilai Negatif Artinya Apa?", "id": "Polaritas Atau Arah Sebenarnya Berlawanan." },
    { "en": "Apakah Rth Bisa Negatif?", "id": "Secara Teoretis Ya, Dengan Sumber Terkendali." },
    { "en": "Apa Artinya Rth Negatif Secara Fisik?", "id": "Rangkaian Tersebut Menyuplai Daya." },
    { "en": "Apa Nama Lain Untuk Sumber Terkendali?", "id": "Sumber Dependen (Dependent Source)." },
    { "en": "Jenis Sumber Terkendali Apa Saja Yang Ada?", "id": "Ada Empat Jenis (VCVS, VCCS, CCVS, CCCS)." },
    { "en": "Apakah Thevenin/Norton Berlaku Untuk Keempat Jenis Itu?", "id": "Ya, Semuanya Adalah Elemen Linear." },
    { "en": "Apa Itu 'Linearity'?", "id": "Hubungan Output Dan Input Proporsional." },
    { "en": "Apa Itu 'Homogeneity'?", "id": "Jika Input Dikalikan Skalar, Output Juga." },
    { "en": "Apa Itu 'Additivity' (Superposisi)?", "id": "Respon Terhadap Jumlah Input Adalah Jumlah Respon." },
    { "en": "Elemen Linear Memenuhi Sifat Apa?", "id": "Homogenitas Dan Aditivitas." },
    { "en": "Apakah Resistor, Induktor, Kapasitor Linear?", "id": "Ya, Idealnya Mereka Linear." },
    { "en": "Kapan Thevenin Lebih Cepat Dari Analisis Mesh?", "id": "Ketika Hanya Butuh Jawaban Di Satu Komponen." },
    { "en": "Apakah Pilihan Ground Mempengaruhi Vth?", "id": "Tidak, Vth Adalah Beda Potensial." },
    { "en": "Apakah Pilihan Ground Mempengaruhi Isc?", "id": "Tidak, Isc Adalah Arus Melalui Cabang." },
    { "en": "Apakah Ekuivalen Thevenin Unik?", "id": "Ya, Untuk Sepasang Terminal Tertentu." },
    { "en": "Jika Terminal Diubah, Apakah Vth/Rth Berubah?", "id": "Ya, Harus Dihitung Ulang." },
    { "en": "Apa Itu 'Port' Dalam Teori Rangkaian?", "id": "Sepasang Terminal Untuk Akses." },
    { "en": "Thevenin/Norton Menganalisis Rangkaian Sebagai 'One-Port Network'?", "id": "Ya, Dilihat Dari Sisi Beban." },
    { "en": "Apa Itu Sirkuit Resiprokal?", "id": "Sirkuit Pasif Linear." },
    { "en": "Apa Itu Teorema Substitusi?", "id": "Cabang Bisa Diganti Sumber Dengan Karakteristik Sama." },
    { "en": "Apa Itu Teorema Millman?", "id": "Menyederhanakan Cabang-cabang Paralel." },
    { "en": "Apakah Teorema Ini Terkait?", "id": "Ya, Semua Adalah Alat Untuk Analisis Rangkaian Linear." },
    { "en": "Apa Langkah Paling Rawan Kesalahan?", "id": "Salah Menghitung Rth, Terutama Dengan Sumber Terkendali." },
    { "en": "Bagaimana Cara Memverifikasi Jawaban?", "id": "Hitung Arus Beban Dengan Metode Lain." },
    { "en": "Contoh Metode Lain?", "id": "Analisis Mesh, Nodal, Atau Superposisi." },
    { "en": "Jika Hasilnya Sama, Berarti Jawabannya?", "id": "Kemungkinan Besar Sudah Benar." },
    { "en": "Apa Keterbatasan Utama Teorema Ini?", "id": "Tidak Berlaku Untuk Rangkaian Non-Linear." },
    { "en": "Apa Keterbatasan Lainnya?", "id": "Tidak Memberikan Informasi Tentang Daya Internal." },
    { "en": "Untuk Menghitung Efisiensi, Perlu Rangkaian Apa?", "id": "Rangkaian Asli Secara Keseluruhan." },
    { "en": "Kesimpulannya, Teorema Ini Adalah Alat Untuk?", "id": "Penyederhanaan Dan Analisis Beban." },
    { "en": "Kunci Untuk Menguasai Teorema Ini?", "id": "Latihan Dengan Berbagai Macam Rangkaian." },
    { "en": "Apa Itu Transformasi Sumber?", "id": "Mengubah Sumber Tegangan Ke Sumber Arus." },
    { "en": "Atau Mengubah Sumber Arus Ke?", "id": "Sumber Tegangan." },
    { "en": "Sumber Tegangan Seri Resistor Bisa Diubah Menjadi Apa?", "id": "Sumber Arus Paralel Resistor." },
    { "en": "Sumber Arus Paralel Resistor Bisa Diubah Menjadi Apa?", "id": "Sumber Tegangan Seri Resistor." },
    { "en": "Bagaimana Nilai Resistor Setelah Transformasi?", "id": "Nilai Resistansinya Tetap Sama." },
    { "en": "Bagaimana Menghitung Arus Ekuivalen (I_eq)?", "id": "I_eq = V_s / R_s." },
    { "en": "Bagaimana Menghitung Tegangan Ekuivalen (V_eq)?", "id": "V_eq = I_s * R_s." },
    { "en": "Apa Hubungan Transformasi Sumber Dengan Thevenin Dan Norton?", "id": "Itu Adalah Proses Mengubah Satu Ke Yang Lain." },
    { "en": "Rangkaian Thevenin Adalah Bentuk Sumber Apa?", "id": "Bentuk Sumber Tegangan Praktis." },
    { "en": "Rangkaian Norton Adalah Bentuk Sumber Apa?", "id": "Bentuk Sumber Arus Praktis." },
    { "en": "Jadi, Mengubah Thevenin Ke Norton Adalah?", "id": "Melakukan Transformasi Sumber." },
    { "en": "Apa Tujuan Melakukan Transformasi Sumber Dalam Analisis?", "id": "Untuk Menyederhanakan Rangkaian Sebelum Analisis Akhir." },
    { "en": "Bisakah Sumber Ideal Ditransformasi?", "id": "Tidak, Harus Ada Resistor Internal." },
    { "en": "Mengapa Sumber Tegangan Ideal Tidak Bisa?", "id": "Karena Resistor Serinya Nol." },
    { "en": "Mengapa Sumber Arus Ideal Tidak Bisa?", "id": "Karena Resistor Paralelnya Tak Terhingga." },
    { "en": "Apakah Transformasi Sumber Mengubah Perilaku Terminal?", "id": "Tidak, Perilaku Terminalnya Tetap Ekuivalen." },
    { "en": "Apa Yang Berubah Saat Transformasi Sumber?", "id": "Rangkaian Internalnya." },
    { "en": "Apakah Tegangan Pada Resistor Rs Sama Setelah Transformasi?", "id": "Tidak, Karena Posisinya Berubah." },
    { "en": "Kapan Transformasi Sumber Sangat Berguna?", "id": "Ketika Sumber Terhubung Secara Campuran (Seri/Paralel)." },
    { "en": "Dapatkah Sumber Terkendali Ditransformasi?", "id": "Ya, Aturannya Sama Saja." },
    { "en": "Variabel Kendali Harus Diperlakukan Bagaimana Saat Transformasi?", "id": "Harus Tetap Dipertahankan Dalam Persamaan." },
    { "en": "Polaritas Sumber Tegangan Baru Ditentukan Oleh Apa?", "id": "Arah Sumber Arus Asli." },
    { "en": "Arah Sumber Arus Baru Ditentukan Oleh Apa?", "id": "Polaritas Sumber Tegangan Asli." },
    { "en": "Jika Tanda Plus Di Atas, Panah Arus Kemana?", "id": "Ke Atas." },
    { "en": "Menonaktifkan Sumber Sama Dengan Mengatur Nilainya Menjadi?", "id": "Nol." },
    { "en": "Sumber Tegangan 0V Ekuivalen Dengan Apa?", "id": "Hubung Singkat." },
    { "en": "Sumber Arus 0A Ekuivalen Dengan Apa?", "id": "Rangkaian Terbuka." },
    { "en": "Logika Ini Menjelaskan Aturan Pencarian Rth?", "id": "Ya, Benar Sekali." },
    { "en": "Apakah Rth Selalu Positif?", "id": "Hanya Jika Rangkaian Tidak Mengandung Sumber Terkendali." },
    { "en": "Saat Mencari Rth, Kita Mencari Resistansi Ekuivalen Apa?", "id": "Resistansi Ekuivalen Pasif." },
    { "en": "Vth Adalah Tegangan Saat Beban Berapa Ohm?", "id": "Saat Beban Tak Terhingga (Terbuka)." },
    { "en": "Isc Adalah Arus Saat Beban Berapa Ohm?", "id": "Saat Beban Nol Ohm (Singkat)." },
    { "en": "Apakah Rangkaian Ekuivalen Bergantung Pada Beban?", "id": "Tidak, Hanya Bergantung Pada Rangkaian Sumber." },
    { "en": "Apa Manfaat Ini?", "id": "Satu Rangkaian Ekuivalen Berlaku Untuk Semua Beban." },
    { "en": "Bagaimana Cara Kerja Voltmeter Ideal?", "id": "Seperti Rangkaian Terbuka (Resistansi Tak Hingga)." },
    { "en": "Pengukuran Voc Mirip Dengan Menggunakan Alat Apa?", "id": "Voltmeter Ideal." },
    { "en": "Bagaimana Cara Kerja Ammeter Ideal?", "id": "Seperti Hubung Singkat (Resistansi Nol)." },
    { "en": "Pengukuran Isc Mirip Dengan Menggunakan Alat Apa?", "id": "Ammeter Ideal." },
    { "en": "Dalam Praktik, Apakah Alat Ukur Ideal?", "id": "Tidak, Memiliki Resistansi Internal." },
    { "en": "Pengukuran Praktis Akan Mempengaruhi Rangkaian?", "id": "Ya, Ada Efek Pembebanan (Loading Effect)." },
    { "en": "Analisis Teoretis Mengasumsikan Alat Ukur Apa?", "id": "Alat Ukur Yang Ideal." },
    { "en": "Jika Dua Rangkaian Thevenin Paralel, Bisakah Disederhanakan?", "id": "Ya, Gunakan Teorema Millman Atau Transformasi." },
    { "en": "Ubah Keduanya Ke Norton, Lalu?", "id": "Jumlahkan Sumber Arus Dan Paralelkan Resistor." },
    { "en": "Bagaimana Jika Dua Rangkaian Thevenin Seri?", "id": "Jumlahkan Sumber Tegangan Dan Resistornya." },
    { "en": "Teorema Thevenin Menyederhanakan Rangkaian Menjadi Berapa Elemen?", "id": "Dua Elemen." },
    { "en": "Teorema Norton Menyederhanakan Rangkaian Menjadi Berapa Elemen?", "id": "Dua Elemen." },
    { "en": "Jika Rangkaian Hanya Terdiri Dari Sumber Independen?", "id": "Maka Rth Adalah Nol." },
    { "en": "Jika Rangkaian Hanya Terdiri Dari Resistor?", "id": "Maka Vth Adalah Nol." },
    { "en": "Metode Mana Yang Membutuhkan Lebih Sedikit Perhitungan?", "id": "Sangat Tergantung Pada Rangkaian." },
    { "en": "Apakah Ada Aturan Pasti Untuk Memilih Metode?", "id": "Tidak, Pilihlah Yang Tampak Paling Mudah." },
    { "en": "Apa Itu 'Driving Point Resistance'?", "id": "Istilah Lain Untuk Resistansi Thevenin." },
    { "en": "Dapatkah Thevenin Digunakan Untuk Menemukan Arus Di Tengah Rangkaian?", "id": "Ya, Dengan Mendefinisikan Ulang Terminal." },
    { "en": "Bagaimana Caranya?", "id": "Potong Rangkaian Di Titik Tersebut." },
    { "en": "Dua Ujung Potongan Menjadi Apa?", "id": "Menjadi Terminal A Dan B." },
    { "en": "Bagian Kiri Menjadi Rangkaian Apa?", "id": "Rangkaian Sumber." },
    { "en": "Bagian Kanan Menjadi Rangkaian Apa?", "id": "Rangkaian Beban." },
    { "en": "Apakah Rangkaian Bisa Memiliki Beberapa Ekuivalen Thevenin?", "id": "Ya, Tergantung Pada Terminal Mana Yang Dipilih." },
    { "en": "Apa Itu 'Output Resistance' Dari Sebuah Penguat?", "id": "Seringkali Sama Dengan Resistansi Theveninnya." },
    { "en": "Apa Itu 'Input Resistance' Dari Sebuah Penguat?", "id": "Resistansi Dilihat Dari Terminal Input." },
    { "en": "Apakah Thevenin Bisa Untuk Mencari 'Input Resistance'?", "id": "Bisa, Dengan Menerapkan Sumber Uji Di Input." },
    { "en": "Inti Dari Teorema Ini Adalah Konsep Apa?", "id": "Konsep Ekuivalensi." },
    { "en": "Apa Manfaat Menggunakan Sumber Uji 1V Atau 1A?", "id": "Perhitungan Menjadi Rth=1/I Atau Rth=V/1." },
    { "en": "Apakah Nilai Sumber Uji Boleh Sembarang?", "id": "Ya, Tapi 1 Adalah Yang Paling Mudah." },
    { "en": "Jika Rangkaian Pasif, Apakah Perlu Sumber Uji?", "id": "Tidak, Cukup Hitung Resistansi Ekuivalen." },
    { "en": "Apa Itu Rangkaian Pasif?", "id": "Rangkaian Tanpa Sumber Energi." },
    { "en": "Rangkaian Pasif Hanya Berisi Apa?", "id": "Resistor, Induktor, Dan Kapasitor." },
    { "en": "Vth Dan In Untuk Rangkaian Pasif?", "id": "Selalu Nol." },
    { "en": "Rangkaian Ekuivalen Rangkaian Pasif Adalah?", "id": "Hanya Sebuah Resistor (Rth)." },
    { "en": "Rth Rangkaian Pasif Selalu Bernilai Apa?", "id": "Positif." },
    { "en": "Apakah Polaritas Sumber Uji Penting?", "id": "Tidak, Karena Resistansi Tidak Memiliki Polaritas." },
    { "en": "Jika Arah Arus Uji Dibalik, Tegangan Uji Bagaimana?", "id": "Polaritasnya Juga Akan Terbalik." },
    { "en": "Rasio V/I Akan Tetap Sama?", "id": "Ya, Tanda Negatif Akan Saling Menghilangkan." },
    { "en": "Dalam Analisis AC, Sumber Uji Menjadi Apa?", "id": "Fasor Tegangan Atau Arus." },
    { "en": "Impedansi Thevenin (Zth) Bisa Memiliki Bagian Imajiner?", "id": "Ya, Jika Rangkaian Mengandung Induktor/Kapasitor." },
    { "en": "Bagian Imajiner Zth Disebut Apa?", "id": "Reaktansi Thevenin (Xth)." },
    { "en": "Bagian Riil Zth Disebut Apa?", "id": "Resistansi Thevenin (Rth)." },
    { "en": "Jadi Zth = ?", "id": "Zth = Rth + jXth." },
    { "en": "Jika Xth Positif, Sifatnya?", "id": "Induktif." },
    { "en": "Jika Xth Negatif, Sifatnya?", "id": "Kapasitif." },
    { "en": "Apa Itu 'Source Degeneration'?", "id": "Teknik Umpan Balik Negatif Pada Penguat." },
    { "en": "Bagaimana Pengaruhnya Terhadap Resistansi Output?", "id": "Meningkatkan Resistansi Output." },
    { "en": "Apakah Thevenin Berguna Untuk Analisis Penguat?", "id": "Sangat Berguna Untuk Menemukan Penguatan Dan Impedansi." },
    { "en": "Apa Itu 'Gain'?", "id": "Rasio Sinyal Output Terhadap Sinyal Input." },
    { "en": "Bagaimana Menghitung 'Gain' Dengan Thevenin?", "id": "Sederhanakan Sisi Input Dan Output Terlebih Dahulu." },
    { "en": "Langkah Menemukan Isc Saat Ada Sumber Terkendali?", "id": "Lakukan Hubung Singkat, Lalu Selesaikan Dengan Nodal/Mesh." },
    { "en": "Apakah Hubung Singkat Mempengaruhi Variabel Kendali?", "id": "Ya, Bisa Mengubahnya Menjadi Nol." },
    { "en": "Jika Variabel Kendali Menjadi Nol, Apa Yang Terjadi?", "id": "Sumber Terkendali Mungkin Menjadi Tidak Aktif." },
    { "en": "Apakah Teorema Thevenin Hanya Satu Langkah?", "id": "Tidak, Seringkali Melibatkan Beberapa Langkah Analisis." },
    { "en": "Teorema Ini Adalah Penyederhanaan Konseptual?", "id": "Ya, Mengubah Masalah Kompleks Menjadi Sederhana." },
    { "en": "Apakah Hasilnya Selalu Rangkaian Seri Atau Paralel?", "id": "Ya, Itu Adalah Bentuk Akhirnya." },
    { "en": "Apakah Berlaku Untuk Sirkuit Digital?", "id": "Bisa, Untuk Menganalisis Sisi Analognya." },
    { "en": "Contohnya?", "id": "Menganalisis 'Driving Strength' Sebuah Gerbang Logika." },
    { "en": "Apa Representasi Thevenin Dari Baterai Nyata?", "id": "Sumber DC Ideal Seri Dengan Resistor Internal." },
    { "en": "Apa Itu 'Internal Resistance'?", "id": "Resistansi Thevenin Dari Baterai." },
    { "en": "Mengapa Tegangan Baterai Turun Saat Diberi Beban?", "id": "Karena Ada Jatuh Tegangan Di Resistansi Internal." },
    { "en": "Apa Itu Sumber Independen?", "id": "Nilainya Tidak Bergantung Pada Rangkaian." },
    { "en": "Apa Itu Sumber Dependen?", "id": "Nilainya Bergantung Pada Tegangan/Arus Lain." },
    { "en": "Simbol Sumber Dependen Berbentuk Apa?", "id": "Berlian Atau Belah Ketupat." },
    { "en": "VCVS Adalah Singkatan Dari?", "id": "Voltage-Controlled Voltage Source." },
    { "en": "VCCS Adalah Singkatan Dari?", "id": "Voltage-Controlled Current Source." },
    { "en": "CCVS Adalah Singkatan Dari?", "id": "Current-Controlled Voltage Source." },
    { "en": "CCCS Adalah Singkatan Dari?", "id": "Current-Controlled Current Source." },
    { "en": "Apa Satuan Penguatan VCVS?", "id": "Tanpa Satuan (Volt/Volt)." },
    { "en": "Apa Satuan Penguatan VCCS?", "id": "Siemens (Ampere/Volt)." },
    { "en": "Apa Satuan Penguatan CCVS?", "id": "Ohm (Volt/Ampere)." },
    { "en": "Apa Satuan Penguatan CCCS?", "id": "Tanpa Satuan (Ampere/Ampere)." },
    { "en": "Saat Mencari Rth, Sumber Dependen Diperlakukan Bagaimana?", "id": "Tetap Dibiarkan Aktif Dalam Rangkaian." },
    { "en": "Metode Apa Yang Tidak Bisa Untuk Mencari Rth Disini?", "id": "Metode Penyederhanaan Seri-Paralel Biasa." },
    { "en": "Dua Metode Apa Yang Bisa Digunakan?", "id": "Sumber Uji Atau Metode Voc/Isc." },
    { "en": "Langkah Pertama Metode Sumber Uji?", "id": "Matikan Semua Sumber Independen." },
    { "en": "Langkah Kedua Metode Sumber Uji?", "id": "Terapkan Sumber Uji (V_t atau I_t) Di Terminal." },
    { "en": "Langkah Ketiga Metode Sumber Uji?", "id": "Hitung Responnya (I_t atau V_t)." },
    { "en": "Langkah Keempat Metode Sumber Uji?", "id": "Hitung Rth = V_t / I_t." },
    { "en": "Apakah Variabel Kendali Perlu Dinyatakan Dalam Variabel Uji?", "id": "Ya, Itu Adalah Kunci Perhitungannya." },
    { "en": "Langkah Pertama Metode Voc/Isc?", "id": "Hitung Voc Seperti Biasa (Semua Sumber Aktif)." },
    { "en": "Langkah Kedua Metode Voc/Isc?", "id": "Hitung Isc Seperti Biasa (Semua Sumber Aktif)." },
    { "en": "Langkah Ketiga Metode Voc/Isc?", "id": "Hitung Rth = Voc / Isc." },
    { "en": "Metode Mana Yang Lebih Disukai?", "id": "Tergantung Preferensi Dan Kemudahan Perhitungan." },
    { "en": "Apa Yang Terjadi Jika Voc Atau Isc Nol?", "id": "Metode Voc/Isc Mungkin Gagal Atau Rumit." },
    { "en": "Jika Rangkaian Hanya Sumber Dependen Dan Resistor?", "id": "Maka Voc Dan Isc Adalah Nol." },
    { "en": "Mengapa Keduanya Nol?", "id": "Karena Tidak Ada Eksitasi Independen." },
    { "en": "Dalam Kasus Ini, Metode Apa Yang Harus Dipakai?", "id": "Harus Menggunakan Metode Sumber Uji." },
    { "en": "Apa Arti Fisik Rth Dengan Sumber Dependen?", "id": "Resistansi Dinamis Yang Dilihat Oleh Beban." },
    { "en": "Apakah Rth Dengan Sumber Dependen Selalu Positif?", "id": "Tidak, Bisa Negatif." },
    { "en": "Rth Negatif Menandakan Rangkaian Bersifat?", "id": "Aktif, Menyuplai Energi." },
    { "en": "Di Mana Sumber Dependen Ditemukan Dalam Praktik?", "id": "Dalam Model Rangkaian Ekuivalen Transistor." },
    { "en": "Transistor BJT Dimodelkan Dengan Sumber Apa?", "id": "Sumber Arus Terkendali Arus (CCCS)." },
    { "en": "Transistor FET/MOSFET Dimodelkan Dengan Sumber Apa?", "id": "Sumber Arus Terkendali Tegangan (VCCS)." },
    { "en": "Op-Amp Ideal Dimodelkan Dengan Sumber Apa?", "id": "Sumber Tegangan Terkendali Tegangan (VCVS)." },
    { "en": "Jadi, Teorema Thevenin Penting Untuk Analisis Apa?", "id": "Analisis Rangkaian Elektronik (Penguat)." },
    { "en": "Resistansi Output Penguat Adalah Resistansi Apa?", "id": "Resistansi Thevenin Dilihat Dari Output." },
    { "en": "Resistansi Input Penguat Adalah Resistansi Apa?", "id": "Resistansi Thevenin Dilihat Dari Input." },
    { "en": "Apakah Beban Mempengaruhi Resistansi Input?", "id": "Bisa, Tergantung Topologi Penguatnya." },
    { "en": "Ketika Menghubungkan Penguat, Apa Yang Terjadi?", "id": "Resistansi Output Satu Tahap Menjadi Sumber Untuk Berikutnya." },
    { "en": "Ini Membentuk Efek Apa?", "id": "Efek Pembebanan (Loading Effect)." },
    { "en": "Thevenin Membantu Menganalisis 'Loading Effect'?", "id": "Ya, Sangat Membantu." },
    { "en": "Resistansi Input Ideal Penguat Tegangan?", "id": "Tak Terhingga." },
    { "en": "Resistansi Output Ideal Penguat Tegangan?", "id": "Nol." },
    { "en": "Saat Menerapkan Sumber Uji, Apa Yang Dicari?", "id": "Hubungan Antara Tegangan Dan Arus Di Terminal." },
    { "en": "Hubungan V-I Di Terminal Itulah Rth?", "id": "Ya, Benar." },
    { "en": "Apakah Sumber Uji Mempengaruhi Sumber Dependen?", "id": "Ya, Secara Tidak Langsung Melalui Variabel Kendali." },
    { "en": "Perhitungan Vth Dengan Sumber Dependen Bagaimana?", "id": "Gunakan Analisis Rangkaian Biasa (Nodal/Mesh)." },
    { "en": "Apakah Sumber Dependen Mempengaruhi Nilai Vth?", "id": "Ya, Tentu Saja." },
    { "en": "Perhitungan Isc Dengan Sumber Dependen Bagaimana?", "id": "Sama, Gunakan Nodal/Mesh Pada Rangkaian Hubung Singkat." },
    { "en": "Apakah Thevenin/Norton Menyederhanakan Jumlah Sumber Dependen?", "id": "Tidak, Efeknya Tergabung Dalam Vth Dan Rth." },
    { "en": "Dapatkah Rangkaian Ekuivalen Memiliki Sumber Dependen?", "id": "Tidak, Bentuk Akhirnya Selalu Sumber Independen." },
    { "en": "Apa Yang Terjadi Jika Variabel Kendali Di Luar Rangkaian?", "id": "Ini Bukan Lagi Rangkaian 'One-Port'." },
    { "en": "Teorema Ini Hanya Berlaku Untuk Rangkaian 'One-Port'?", "id": "Ya, Dilihat Dari Terminal Beban." },
    { "en": "Jika Rangkaian Tidak Punya Sumber Independen, Vth/In Berapa?", "id": "Selalu Nol." },
    { "en": "Rangkaian Ekuivalennya Menjadi Apa?", "id": "Hanya Sebuah Resistor (Rth)." },
    { "en": "Jadi, Rangkaian Pasif Ekuivalen Dengan Apa?", "id": "Sebuah Resistor Tunggal." },
    { "en": "Dan Rangkaian Aktif Tanpa Input Independen?", "id": "Juga Ekuivalen Dengan Sebuah Resistor." },
    { "en": "Resistor Ini Bisa Bernilai Negatif?", "id": "Ya, Jika Aktif." },
    { "en": "Apakah Ada Batas Kompleksitas Rangkaian?", "id": "Tidak, Selama Rangkaiannya Linear." },
    { "en": "Semakin Kompleks, Perhitungan Semakin?", "id": "Semakin Sulit Dan Panjang." },
    { "en": "Perangkat Lunak Simulasi Menggunakan Teorema Ini?", "id": "Tidak Secara Langsung, Mereka Menggunakan Analisis Matriks." },
    { "en": "Contoh Perangkat Lunak Simulasi?", "id": "SPICE (Simulation Program with Integrated Circuit Emphasis)." },
    { "en": "Namun, Teorema Ini Penting Untuk Pemahaman Konseptual?", "id": "Sangat Penting." },
    { "en": "Apakah Menghubung Singkat Terminal Bisa Merusak Rangkaian Nyata?", "id": "Ya, Bisa Sangat Berbahaya." },
    { "en": "Perhitungan Isc Adalah Proses Teoretis?", "id": "Ya, Bukan Sesuatu Yang Selalu Dilakukan Di Lab." },
    { "en": "Di Lab, Bagaimana Cara Menemukan Rth?", "id": "Menggunakan Metode Pengukuran Voc Dan Arus Beban." },
    { "en": "Apa Itu 'Maximum Power Transfer Theorem'?", "id": "Teorema Transfer Daya Maksimum." },
    { "en": "Berlaku Juga Untuk Rangkaian Dengan Sumber Dependen?", "id": "Ya, Aturannya Tetap Sama (RL = Rth)." },
    { "en": "Jika Rth Negatif, Apa Artinya Daya Maksimum?", "id": "Rangkaian Bisa Memberi Daya Tak Terhingga (Tidak Stabil)." },
    { "en": "Rangkaian Seperti Ini Disebut Rangkaian Apa?", "id": "Rangkaian Tidak Stabil." },
    { "en": "Contoh Rangkaian Tidak Stabil?", "id": "Osilator." },
    { "en": "Jadi Thevenin Bisa Memprediksi Ketidakstabilan?", "id": "Ya, Jika Rth Ditemukan Negatif." },
    { "en": "Apa Langkah Terpenting Saat Menganalisis Sumber Dependen?", "id": "Menulis Persamaan Kendali Dengan Benar." },
    { "en": "Kesalahan Di Persamaan Kendali Akan Menyebabkan Apa?", "id": "Hasil Akhir Yang Salah Total." },
    { "en": "Apakah Sumber Dependen Mematuhi Hukum Ohm?", "id": "Tidak, Mereka Adalah Sumber Ideal." },
    { "en": "Apakah Mereka Mematuhi Hukum Kirchhoff?", "id": "Ya, Semua Elemen Rangkaian Mematuhinya." },
    { "en": "Apa Itu 'Small-Signal Model'?", "id": "Model Linear Dari Perangkat Non-Linear." },
    { "en": "Di Model Inilah Sumber Dependen Muncul?", "id": "Ya, Benar." },
    { "en": "Teorema Thevenin Adalah Fondasi Untuk Apa?", "id": "Analisis Rangkaian Elektronik Tingkat Lanjut." },
    { "en": "Apakah Thevenin/Norton Bisa Diterapkan Berulang Kali?", "id": "Ya, Untuk Menyederhanakan Rangkaian Secara Bertahap." },
    { "en": "Apakah Perlu Menggambar Ulang Rangkaian?", "id": "Seringkali Sangat Membantu Visualisasi." },
    { "en": "Jika Tegangannya 5Vx Dan Variabel Kendali Vx Di Hubung Singkat?", "id": "Maka Vx = 0, Sumber Menjadi 0V (Hubung Singkat)." },
    { "en": "Jika Arusnya 2Ix Dan Cabang Ix Dibuka?", "id": "Maka Ix = 0, Sumber Menjadi 0A (Rangkaian Terbuka)." },
    { "en": "Apa Itu 'Gyrator'?", "id": "Elemen Dua Port Yang Mengubah Impedansi." },
    { "en": "Bagaimana 'Gyrator' Dimodelkan?", "id": "Menggunakan Sumber Terkendali." },
    { "en": "'Gyrator' Bisa Membuat Kapasitor Terlihat Seperti Induktor?", "id": "Ya, Itu Adalah Salah Satu Aplikasinya." },
    { "en": "Apa Itu 'Negative Impedance Converter' (NIC)?", "id": "Rangkaian Yang Menghasilkan Impedansi Negatif." },
    { "en": "NIC Dibuat Dengan Apa?", "id": "Op-Amp Dan Resistor." },
    { "en": "NIC Adalah Contoh Praktis Rangkaian Dengan Rth Negatif?", "id": "Ya, Benar Sekali." },
    { "en": "Apa Itu Model Hibrida-pi Transistor?", "id": "Model Sinyal Kecil Yang Menggunakan Sumber Dependen." },
    { "en": "Sumber Apa Yang Ada Di Model Hibrida-pi?", "id": "Sumber Arus Terkendali Tegangan (VCCS)." },
    { "en": "Analisis Penguat Mengandalkan Penyederhanaan Model Ini?", "id": "Ya, Menggunakan Teorema Thevenin." },
    { "en": "Apakah Konsep Ini Hanya Untuk Teknik Elektro?", "id": "Tidak, Konsep Ekuivalensi Digunakan Di Banyak Bidang." },
    { "en": "Contoh Bidang Lain?", "id": "Sistem Mekanis, Akustik, Termal." },
    { "en": "Setiap Sistem Linear Bisa Disederhanakan?", "id": "Ya, Menjadi Bentuk Ekuivalennya." },
    { "en": "Apa Itu Impedansi (Z)?", "id": "Ukuran Hambatan Total Terhadap Arus AC." },
    { "en": "Impedansi Adalah Besaran Kompleks?", "id": "Ya, Memiliki Bagian Riil Dan Imajiner." },
    { "en": "Bagian Riil Impedansi Disebut Apa?", "id": "Resistansi (R)." },
    { "en": "Bagian Imajiner Impedansi Disebut Apa?", "id": "Reaktansi (X)." },
    { "en": "Jadi, Z = ?", "id": "Z = R + jX." },
    { "en": "Apakah Teorema Thevenin Berlaku Untuk Impedansi?", "id": "Ya, Sepenuhnya Berlaku." },
    { "en": "Resistansi Thevenin (Rth) Menjadi Apa?", "id": "Impedansi Thevenin (Zth)." },
    { "en": "Tegangan Thevenin (Vth) Menjadi Apa?", "id": "Fasor Tegangan Thevenin (Vth)." },
    { "en": "Arus Norton (In) Menjadi Apa?", "id": "Fasor Arus Norton (In)." },
    { "en": "Hubungannya Menjadi Vth = ?", "id": "Vth = In * Zth." },
    { "en": "Bagaimana Cara Mencari Vth Dalam Rangkaian AC?", "id": "Gunakan Analisis Fasor (Nodal/Mesh) Di Terminal Terbuka." },
    { "en": "Bagaimana Cara Mencari In Dalam Rangkaian AC?", "id": "Gunakan Analisis Fasor Di Terminal Hubung Singkat." },
    { "en": "Bagaimana Cara Mencari Zth?", "id": "Matikan Sumber Independen, Cari Impedansi Ekuivalen." },
    { "en": "Bagaimana Impedansi Resistor?", "id": "Z_R = R." },
    { "en": "Bagaimana Impedansi Induktor?", "id": "Z_L = jωL." },
    { "en": "Bagaimana Impedansi Kapasitor?", "id": "Z_C = 1 / (jωC) = -j / (ωC)." },
    { "en": "Apa Itu ω (Omega)?", "id": "Frekuensi Sudut (2πf)." },
    { "en": "Apakah ω Mempengaruhi Nilai Zth?", "id": "Ya, Jika Ada Induktor Atau Kapasitor." },
    { "en": "Zth Adalah Fungsi Dari Apa?", "id": "Fungsi Dari Frekuensi." },
    { "en": "Bagaimana Impedansi Seri Dijumlahkan?", "id": "Z_eq = Z1 + Z2." },
    { "en": "Bagaimana Impedansi Paralel Dijumlahkan?", "id": "Seperti Resistor Paralel (Menggunakan Invers)." },
    { "en": "Apa Itu Admitansi (Y)?", "id": "Kebalikan Dari Impedansi (Y = 1/Z)." },
    { "en": "Menjumlahkan Admitansi Paralel Bagaimana?", "id": "Y_eq = Y1 + Y2." },
    { "en": "Jika Rangkaian AC Memiliki Sumber Dependen?", "id": "Gunakan Metode Sumber Uji Atau Voc/Isc." },
    { "en": "Aturannya Sama Persis Dengan Rangkaian DC?", "id": "Ya, Hanya Saja Perhitungannya Menggunakan Bilangan Kompleks." },
    { "en": "Sumber Uji Yang Diterapkan Berupa Apa?", "id": "Fasor Tegangan Atau Arus." },
    { "en": "Zth Dihitung Sebagai Rasio Apa?", "id": "Rasio Fasor Tegangan Uji Terhadap Fasor Arus Uji." },
    { "en": "Apa Itu Teorema Transfer Daya Maksimum Rata-rata?", "id": "Versi AC Dari Teorema Daya Maksimum." },
    { "en": "Kapan Daya Rata-rata Maksimum Ditransfer?", "id": "Ketika Impedansi Beban Adalah Konjugat Zth." },
    { "en": "Syaratnya: Z_L = ?", "id": "Z_L = Zth*." },
    { "en": "Apa Itu Konjugat Kompleks?", "id": "Ubah Tanda Bagian Imajiner." },
    { "en": "Jika Zth = Rth + jXth, Maka Z_L = ?", "id": "Z_L = Rth - jXth." },
    { "en": "Ini Berarti Resistansi Beban Harus Sama Dengan?", "id": "Resistansi Thevenin (R_L = Rth)." },
    { "en": "Dan Reaktansi Beban Harus Sama Dengan?", "id": "Negatif Dari Reaktansi Thevenin (X_L = -Xth)." },
    { "en": "Apa Tujuan Membuat X_L = -Xth?", "id": "Untuk Membuat Reaktansi Total Rangkaian Menjadi Nol." },
    { "en": "Kondisi Ini Disebut Apa?", "id": "Resonansi." },
    { "en": "Jadi, Daya Maksimum Terjadi Saat Resonansi?", "id": "Ya, Dan Ketika Resistansinya Cocok." },
    { "en": "Beban Apa Yang Bisa Memiliki Reaktansi Negatif?", "id": "Kapasitor." },
    { "en": "Beban Apa Yang Bisa Memiliki Reaktansi Positif?", "id": "Induktor." },
    { "en": "Jika Zth Induktif, Beban Harus Bersifat?", "id": "Kapasitif." },
    { "en": "Jika Zth Kapasitif, Beban Harus Bersifat?", "id": "Induktif." },
    { "en": "Jika Beban Terbatas Hanya Resistor Variabel, Syarat Pmax?", "id": "R_L = |Zth| (Magnitudo Impedansi Thevenin)." },
    { "en": "Apa Itu 'Pencocokan Impedansi'?", "id": "Proses Menyesuaikan Z_L Agar Sama Dengan Zth*." },
    { "en": "Alat Apa Yang Digunakan Untuk 'Impedance Matching'?", "id": "Jaringan Pencocokan (Matching Network)." },
    { "en": "Jaringan Pencocokan Biasanya Terdiri Dari Apa?", "id": "Induktor Dan Kapasitor." },
    { "en": "Di Mana 'Impedance Matching' Sangat Penting?", "id": "Sistem Frekuensi Radio (RF)." },
    { "en": "Contohnya?", "id": "Menghubungkan Antena Ke Pemancar." },
    { "en": "Apa Itu 'Voltage Standing Wave Ratio' (VSWR)?", "id": "Ukuran Ketidakcocokan Impedansi." },
    { "en": "VSWR Ideal Bernilai Berapa?", "id": "Satu Banding Satu (1:1)." },
    { "en": "VSWR 1:1 Berarti Pencocokan Sempurna?", "id": "Ya, Benar." },
    { "en": "Apakah Vth Bisa Berupa Bilangan Kompleks?", "id": "Ya, Memiliki Magnitudo Dan Sudut Fasa." },
    { "en": "Apakah In Bisa Berupa Bilangan Kompleks?", "id": "Ya, Sama Seperti Vth." },
    { "en": "Apakah Zth Bisa Berupa Bilangan Kompleks?", "id": "Ya, Tentu Saja." },
    { "en": "Perhitungan Dengan Bilangan Kompleks Bisa Dilakukan Dalam Bentuk Apa?", "id": "Bentuk Rectangular (a+jb) Atau Polar (r∠θ)." },
    { "en": "Bentuk Apa Yang Mudah Untuk Penjumlahan/Pengurangan?", "id": "Bentuk Rectangular." },
    { "en": "Bentuk Apa Yang Mudah Untuk Perkalian/Pembagian?", "id": "Bentuk Polar." },
    { "en": "Saat Mencari Zth, Impedansi Seri/Paralel Melibatkan Apa?", "id": "Penjumlahan Dan Perkalian Kompleks." },
    { "en": "Apa Itu 'Phasor Diagram'?", "id": "Representasi Grafis Dari Fasor." },
    { "en": "Apakah Thevenin Bisa Dilihat Di 'Phasor Diagram'?", "id": "Ya, Sebagai Vektor Tegangan." },
    { "en": "Apa Itu Transformator?", "id": "Mengubah Level Tegangan AC Menggunakan Induksi." },
    { "en": "Bisakah Thevenin Digunakan Di Sisi Sekunder Trafo?", "id": "Ya, Sangat Umum Digunakan." },
    { "en": "Apa Itu 'Reflected Impedance'?", "id": "Impedansi Di Sisi Sekunder Yang Terlihat Dari Primer." },
    { "en": "Teorema Thevenin Bisa Diterapkan Pada Frekuensi Berapa Pun?", "id": "Ya, Tapi Zth Akan Berubah Dengan Frekuensi." },
    { "en": "Apa Itu 'Bode Plot'?", "id": "Grafik Respon Frekuensi (Magnitudo Dan Fasa)." },
    { "en": "Bisakah Thevenin Digunakan Untuk Menemukan Fungsi Transfer?", "id": "Ya, Vth(s) / V_in(s)." },
    { "en": "Apa Itu Domain-s?", "id": "Domain Frekuensi Kompleks (Analisis Laplace)." },
    { "en": "Dalam Domain-s, jω Diganti Dengan Apa?", "id": "Variabel Kompleks 's'." },
    { "en": "Impedansi Induktor Di Domain-s?", "id": "sL." },
    { "en": "Impedansi Kapasitor Di Domain-s?", "id": "1 / (sC)." },
    { "en": "Analisis Thevenin Di Domain-s Berguna Untuk Apa?", "id": "Menganalisis Respon Transien Dan Stabilitas." },
    { "en": "Stabilitas Ditentukan Oleh Apa?", "id": "Posisi Pole Dari Fungsi Transfer." },
    { "en": "Pole Fungsi Transfer Terkait Dengan Apa?", "id": "Akar Dari Denominator Zth(s)." },
    { "en": "Apa Itu Filter Pasif?", "id": "Filter Yang Hanya Menggunakan R, L, C." },
    { "en": "Bagaimana Menganalisis Filter Pasif?", "id": "Gunakan Thevenin Untuk Menemukan Tegangan Output." },
    { "en": "Rangkaian Thevenin Filter Lolos-Rendah RC?", "id": "Vth Adalah Pembagi Tegangan, Zth Adalah R || (1/sC)." },
    { "en": "Apakah Teorema Ini Tetap Sama Secara Fundamental?", "id": "Ya, Hanya Matematikanya Yang Menjadi Lebih Abstrak." },
    { "en": "Apa Itu 'Resonance'?", "id": "Fenomena Osilasi Amplitudo Besar." },
    { "en": "Bagaimana Thevenin Berperilaku Di Dekat Frekuensi Resonansi?", "id": "Magnitudo Zth Bisa Berubah Sangat Drastis." },
    { "en": "Pada Resonansi Seri, Zth Menjadi?", "id": "Minimum Dan Murni Resistif." },
    { "en": "Pada Resonansi Paralel, Zth Menjadi?", "id": "Maksimum Dan Murni Resistif." },
    { "en": "Apa Itu 'Q Factor'?", "id": "Faktor Kualitas, Ukuran Ketajaman Resonansi." },
    { "en": "Q Tinggi Berarti Zth Berubah Bagaimana?", "id": "Berubah Sangat Cepat Di Sekitar Resonansi." },
    { "en": "Apa Itu 'Bandwidth'?", "id": "Rentang Frekuensi Operasi Efektif." },
    { "en": "Bandwidth Terkait Dengan Q Factor?", "id": "Ya, Berbanding Terbalik." },
    { "en": "Apakah Teori Daya Maksimum AC Memerlukan Pengetahuan Thevenin?", "id": "Ya, Itu Adalah Prasyaratnya." },
    { "en": "Apa Itu 'Power Factor Correction'?", "id": "Memperbaiki Faktor Daya Agar Mendekati Satu." },
    { "en": "Bagaimana Caranya?", "id": "Menambahkan Reaktansi Berlawanan (Kapasitor) Secara Paralel." },
    { "en": "Apakah Thevenin Berguna Untuk Menganalisis Sistem Tiga Fasa?", "id": "Ya, Dalam Analisis Per-Fasa." },
    { "en": "Setiap Fasa Dapat Disederhanakan Menjadi Apa?", "id": "Rangkaian Ekuivalen Thevenin." },
    { "en": "Apa Itu 'Per-Unit System'?", "id": "Sistem Normalisasi Dalam Analisis Sistem Tenaga." },
    { "en": "Thevenin Bisa Digunakan Dalam Sistem Per-Unit?", "id": "Ya, Sangat Umum." },
    { "en": "Apa Itu Transfer Daya?", "id": "Pengiriman Energi Dari Sumber Ke Beban." },
    { "en": "Apa Tujuan Utama Transfer Daya?", "id": "Menyuplai Energi Agar Beban Bisa Bekerja." },
    { "en": "Apa Yang Membatasi Transfer Daya?", "id": "Resistansi Internal Sumber (Rth)." },
    { "en": "Teorema Apa Yang Membahas Ini?", "id": "Teorema Transfer Daya Maksimum." },
    { "en": "Apa Pernyataan Teorema Ini Untuk DC?", "id": "Daya Maksimum Terjadi Saat RL = Rth." },
    { "en": "RL Adalah Resistansi Apa?", "id": "Resistansi Beban (Load)." },
    { "en": "Rth Adalah Resistansi Apa?", "id": "Resistansi Thevenin Dari Sumber." },
    { "en": "Bagaimana Daya Beban Jika RL Sangat Kecil?", "id": "Daya Kecil (Arus Tinggi, Tegangan Rendah)." },
    { "en": "Bagaimana Daya Beban Jika RL Sangat Besar?", "id": "Daya Kecil (Tegangan Tinggi, Arus Rendah)." },
    { "en": "Di Mana Titik Puncak Kurva Daya?", "id": "Tepat Saat RL = Rth." },
    { "en": "Grafik Daya Beban Terhadap Resistansi Beban Berbentuk Apa?", "id": "Berbentuk Seperti Lonceng Atau Bukit." },
    { "en": "Berapa Daya Maksimum (Pmax) Yang Bisa Ditransfer?", "id": "Pmax = (Vth)² / (4 * Rth)." },
    { "en": "Daya Ini Ditransfer Ke Mana?", "id": "Ke Komponen Beban (RL)." },
    { "en": "Berapa Daya Yang Hilang Di Sumber?", "id": "Sama Dengan Daya Yang Ditransfer Ke Beban." },
    { "en": "Jadi Total Daya Yang Dihasilkan Sumber?", "id": "Dua Kali Pmax." },
    { "en": "Berapa Efisiensi Saat Transfer Daya Maksimum?", "id": "Lima Puluh Persen." },
    { "en": "Rumus Efisiensi (η)?", "id": "η = (Daya Beban / Daya Total) * 100%." },
    { "en": "Mengapa Efisiensinya 50%?", "id": "Karena Setengah Daya Hilang Di Rth." },
    { "en": "Kapan Kondisi Daya Maksimum Diinginkan?", "id": "Ketika Daya Sinyal Sangat Penting." },
    { "en": "Contoh Aplikasi?", "id": "Penerima Radio, Amplifier Audio, Komunikasi." },
    { "en": "Mengapa Penting Di Penerima Radio?", "id": "Untuk Mendapatkan Sinyal Terkuat Dari Antena." },
    { "en": "Impedansi Antena Harus Cocok Dengan Apa?", "id": "Impedansi Input Penerima." },
    { "en": "Kapan Kondisi Daya Maksimum Tidak Diinginkan?", "id": "Ketika Efisiensi Adalah Prioritas Utama." },
    { "en": "Contoh Aplikasi?", "id": "Sistem Transmisi Tenaga Listrik (PLN)." },
    { "en": "Apa Target Utama Sistem Tenaga?", "id": "Mengirim Daya Dengan Kerugian Minimal." },
    { "en": "Bagaimana Cara Mencapai Efisiensi Tinggi?", "id": "Buat RL Jauh Lebih Besar Dari Rth." },
    { "en": "Resistansi Internal (Rth) Jaringan Listrik Diusahakan Bagaimana?", "id": "Sangat Kecil." },
    { "en": "Resistansi Beban (Rumah, Industri) Bagaimana?", "id": "Jauh Lebih Besar Dari Resistansi Jaringan." },
    { "en": "Apa Itu 'Impedance Matching'?", "id": "Proses Mencocokkan Impedansi Beban Dengan Sumber." },
    { "en": "Tujuannya Bisa Untuk Daya Maksimum?", "id": "Ya." },
    { "en": "Tujuannya Bisa Untuk Mencegah Refleksi?", "id": "Ya, Di Saluran Transmisi." },
    { "en": "Bagaimana Aturan Daya Maksimum Untuk Sumber AC?", "id": "Impedansi Beban Harus Konjugat Kompleks Sumber." },
    { "en": "Jadi, ZL = ?", "id": "ZL = Zth*." },
    { "en": "Jika Zth = 10 + j5 Ohm, ZL Harus?", "id": "ZL = 10 - j5 Ohm." },
    { "en": "Apa Yang Dicocokkan Dalam Pencocokan Konjugat?", "id": "Resistansi Disamakan, Reaktansi Dihilangkan." },
    { "en": "Menghilangkan Reaktansi Sama Dengan Menciptakan Kondisi Apa?", "id": "Kondisi Resonansi." },
    { "en": "Apa Yang Terjadi Jika Hanya Resistansi Beban (RL) Yang Bisa Diubah?", "id": "RL Harus Sama Dengan Magnitudo Zth." },
    { "en": "Syaratnya: RL = ?", "id": "RL = |Zth| = √(Rth² + Xth²)." },
    { "en": "Apakah Thevenin/Norton Diperlukan Untuk Teorema Ini?", "id": "Ya, Untuk Menemukan Vth Dan Rth/Zth." },
    { "en": "Jadi Ini Adalah Aplikasi Langsung Dari Teorema Thevenin?", "id": "Betul Sekali." },
    { "en": "Apakah Daya Maksimum Bergantung Pada Vth?", "id": "Ya, Sebanding Dengan Kuadrat Vth." },
    { "en": "Semakin Besar Vth, Daya Maksimum Semakin?", "id": "Semakin Besar." },
    { "en": "Apakah Daya Maksimum Bergantung Pada Rth?", "id": "Ya, Berbanding Terbalik Dengan Rth." },
    { "en": "Semakin Kecil Rth, Daya Maksimum Semakin?", "id": "Semakin Besar." },
    { "en": "Bisakah Teorema Ini Dibuktikan Dengan Kalkulus?", "id": "Ya, Dengan Mencari Turunan Dan Menyamakannya Dengan Nol." },
    { "en": "Turunan Apa Yang Dicari?", "id": "Turunan Daya Beban Terhadap Resistansi Beban." },
    { "en": "Apakah Teorema Ini Bekerja Untuk Ekuivalen Norton?", "id": "Ya, Tentu Saja." },
    { "en": "Dalam Bentuk Norton, RL = ?", "id": "RL = Rn (Karena Rn = Rth)." },
    { "en": "Rumus Pmax Dalam Bentuk Norton?", "id": "Pmax = (In)² * Rn / 4." },
    { "en": "Apakah Daya Yang Hilang Di Rn Juga Sama?", "id": "Ya, Daya Yang Hilang Juga Pmax." },
    { "en": "Apa Itu 'Matching Network'?", "id": "Jaringan (Biasanya LC) Untuk Mencocokkan Impedansi." },
    { "en": "Di Mana Ini Digunakan?", "id": "Di Antara Sumber Dan Beban." },
    { "en": "Fungsi 'Matching Network'?", "id": "Membuat Beban Terlihat Seperti Konjugat Sumber." },
    { "en": "Apa Itu 'Antenna Tuner'?", "id": "Contoh 'Matching Network' Variabel." },
    { "en": "Apa Yang Dilakukan Speaker Mobil Saat Dayanya Ditulis 'Max'?", "id": "Itu Adalah Daya Puncak, Bukan Daya Rata-rata." },
    { "en": "Impedansi Speaker (Misal 4 Ohm) Harus Cocok Dengan Apa?", "id": "Impedansi Output Amplifier." },
    { "en": "Ini Untuk Transfer Daya Maksimum?", "id": "Ya, Dan Untuk Mencegah Kerusakan Amplifier." },
    { "en": "Apa Yang Terjadi Jika Baterai Dihubung Singkat?", "id": "Arus Sangat Besar, Daya Maksimal Hilang Di Baterai." },
    { "en": "Baterai Menjadi Sangat Panas?", "id": "Ya, Karena Disipasi Daya Internal (I² * Rth)." },
    { "en": "Resistansi Internal Sel Surya Penting?", "id": "Sangat Penting Untuk Efisiensi." },
    { "en": "Beban Sel Surya Harus Dicocokkan?", "id": "Ya, Untuk Mendapatkan Daya Maksimal." },
    { "en": "Proses Ini Disebut Apa Dalam Fotovoltaik?", "id": "Maximum Power Point Tracking (MPPT)." },
    { "en": "Apa Itu MPPT?", "id": "Sistem Elektronik Yang Terus Menyesuaikan Beban." },
    { "en": "Mengapa Perlu Disesuaikan Terus Menerus?", "id": "Karena Karakteristik Sel Surya Berubah Dengan Cahaya." },
    { "en": "Apakah Resistansi Thevenin Bergantung Pada Beban?", "id": "Tidak, Itu Adalah Sifat Dari Rangkaian Sumber." },
    { "en": "Apakah Tegangan Thevenin Bergantung Pada Beban?", "id": "Tidak." },
    { "en": "Apakah Daya Yang Ditransfer Bergantung Pada Beban?", "id": "Ya, Sangat Bergantung." },
    { "en": "Jadi, Vth Dan Rth Adalah Konstanta?", "id": "Ya, Untuk Rangkaian Sumber Yang Diberikan." },
    { "en": "Dan RL Adalah Variabel?", "id": "Ya, Dalam Konteks Teorema Ini." },
    { "en": "Teorema Ini Mengoptimalkan Nilai Apa?", "id": "Nilai RL Untuk Mendapatkan P_L Maksimum." },
    { "en": "Apa Syarat Daya Maksimum Jika Beban Murni Reaktif?", "id": "Daya Rata-rata Yang Ditransfer Selalu Nol." },
    { "en": "Bisakah Teorema Thevenin Diterapkan Pada Transformator?", "id": "Ya, Untuk Menemukan Impedansi Ekuivalennya." },
    { "en": "Teorema Ini Adalah Konsep Kunci Dalam Desain Apa?", "id": "Desain Amplifier Frekuensi Radio (RF)." },
    { "en": "Apa Beda Daya Puncak Dan Daya Rata-rata?", "id": "Daya Puncak Sesaat, Daya Rata-rata Selama Periode." },
    { "en": "Teorema Ini Membahas Daya Apa?", "id": "Daya Rata-rata (Untuk AC) Atau Daya DC." },
    { "en": "Bagaimana Jika Rth Nol (Sumber Ideal)?", "id": "Secara Teori, Bisa Memberi Daya Tak Terhingga." },
    { "en": "Apakah Sumber Ideal Ada Dalam Kenyataan?", "id": "Tidak, Semua Sumber Nyata Memiliki Resistansi Internal." },
    { "en": "Resistansi Internal Membatasi Apa?", "id": "Arus Maksimum Dan Daya Maksimum." },
    { "en": "Apakah Perlu Mengetahui Rangkaian Internal Untuk Teorema Ini?", "id": "Tidak, Cukup Mengetahui Ekuivalen Theveninnya." },
    { "en": "Ini Adalah Keuntungan Besar Dari Teorema Apa?", "id": "Teorema Thevenin." },
    { "en": "Bagaimana Mengukur Rth Baterai?", "id": "Ukur Voc, Lalu Ukur V_L Dengan Beban R_L." },
    { "en": "Dari Situ, Rth Bisa Dihitung?", "id": "Ya, Rth = R_L * (Voc/V_L - 1)." },
    { "en": "Apakah Rth Baterai Konstan?", "id": "Tidak, Berubah Tergantung Tingkat Muatan Dan Usia." },
    { "en": "Apa Yang Terjadi Pada Rth Baterai Saat Habis?", "id": "Nilai Rth Meningkat Drastis." },
    { "en": "Itulah Mengapa Tegangan Baterai Habis Langsung Jatuh?", "id": "Ya, Saat Diberi Beban Sedikit Pun." },
    { "en": "Apakah Teorema Thevenin Bergantung Pada Bentuk Gelombang?", "id": "Untuk AC, Biasanya Diasumsikan Sinusoidal." },
    { "en": "Jika Non-Sinusoidal?", "id": "Analisis Menjadi Lebih Kompleks (Menggunakan Deret Fourier)." },
    { "en": "Setiap Harmonisa Akan Memiliki Ekuivalen Theveninnya Sendiri?", "id": "Ya, Benar." },
    { "en": "Apakah Transfer Daya Maksimum Terjadi Pada Frekuensi Sama?", "id": "Ya, Pada Frekuensi Fundamental." },
    { "en": "Apa Itu 'Insertion Loss'?", "id": "Kehilangan Daya Akibat Penambahan Komponen." },
    { "en": "Pencocokan Impedansi Bertujuan Meminimalkan Apa?", "id": "Meminimalkan 'Insertion Loss' Dan Refleksi." },
    { "en": "Apa Itu 'Return Loss'?", "id": "Ukuran Daya Yang Direfleksikan Kembali Ke Sumber." },
    { "en": "'Return Loss' Tinggi Berarti Pencocokan Bagaimana?", "id": "Pencocokan Yang Baik." },
    { "en": "Jadi Thevenin Adalah Fondasi Dari Banyak Konsep Lanjutan?", "id": "Ya, Terutama Di Bidang RF Dan Komunikasi." },
    { "en": "Apa Analogi Mekanis Untuk Rangkaian Thevenin?", "id": "Pegas (Vth) Dengan Peredam (Rth)." },
    { "en": "Apa Analogi Untuk Beban?", "id": "Massa Yang Ingin Digerakkan." },
    { "en": "Thevenin Berguna Untuk Menganalisis Sensor?", "id": "Ya, Untuk Memodelkan Sensor Dan Antarmukanya." },
    { "en": "Sensor Sering Dimodelkan Sebagai Apa?", "id": "Sumber Tegangan Thevenin." },
    { "en": "Rth Sensor Merepresentasikan Apa?", "id": "Impedansi Output Sensor." },
    { "en": "Apa Itu 'Loading' Pada Sensor?", "id": "Ketika Alat Ukur Mengubah Nilai Yang Diukur." },
    { "en": "Bagaimana Cara Menghindari 'Loading'?", "id": "Gunakan Alat Ukur Dengan Impedansi Input Sangat Tinggi." },
    { "en": "Penguat Instrumentasi Punya Impedansi Input Bagaimana?", "id": "Sangat Tinggi, Untuk Mencegah 'Loading'." },
    { "en": "Jika Rangkaian Hanya Punya Sumber Arus Dan Resistor?", "id": "Lebih Mudah Mencari Ekuivalen Norton Terlebih Dahulu." },
    { "en": "Bagaimana Caranya?", "id": "Gabungkan Sumber Arus Dan Resistor Paralel." },
    { "en": "Kemudian Lakukan Transformasi Sumber Menjadi Thevenin?", "id": "Ya, Jika Bentuk Thevenin Yang Diinginkan." },
    { "en": "Jika Rangkaian Hanya Punya Sumber Tegangan Dan Resistor?", "id": "Lebih Mudah Mencari Ekuivalen Thevenin Langsung." },
    { "en": "Bagaimana Caranya?", "id": "Gabungkan Sumber Tegangan Dan Resistor Seri." },
    { "en": "Apakah Teorema Ini Bisa Diterapkan Secara Grafis?", "id": "Ya, Dengan Menggunakan Kurva Karakteristik V-I." },
    { "en": "Kurva V-I Rangkaian Linear Berbentuk Apa?", "id": "Garis Lurus." },
    { "en": "Titik Potong Sumbu Tegangan Adalah Apa?", "id": "Tegangan Rangkaian Terbuka (Voc)." },
    { "en": "Titik Potong Sumbu Arus Adalah Apa?", "id": "Arus Hubung Singkat (Isc)." },
    { "en": "Kemiringan Garis Adalah Apa?", "id": "Negatif Dari Kebalikan Resistansi Thevenin (-1/Rth)." },
    { "en": "Metode Grafis Berguna Untuk Menganalisis Apa?", "id": "Rangkaian Non-Linear Dengan Garis Beban." },
    { "en": "Apa Itu Garis Beban (Load Line)?", "id": "Grafik V-I Dari Beban." },
    { "en": "Titik Operasi Rangkaian Adalah Perpotongan Apa?", "id": "Kurva Sumber Dan Garis Beban." },
    { "en": "Apakah Thevenin Masih Berguna Di Era Komputer?", "id": "Ya, Untuk Pemahaman Konseptual Dan Desain Awal." },
    { "en": "Membantu Insinyur Untuk Melakukan Apa?", "id": "Melakukan Perkiraan Cepat Tanpa Simulasi." },
    { "en": "Apa Nama Lain Untuk Teorema Thevenin?", "id": "Teorema Helmholtz-Thévenin." },
    { "en": "Mengapa Ada Nama Helmholtz?", "id": "Hermann Von Helmholtz Menemukannya Lebih Dulu." },
    { "en": "Untuk Apa Helmholtz Menggunakannya?", "id": "Untuk Menganalisis Jaringan Biologis (Saraf)." },
    { "en": "Thevenin Menemukannya Secara Independen?", "id": "Ya, Untuk Analisis Rangkaian Telegraf." },
    { "en": "Apakah Ekuivalen Norton Juga Punya Nama Lain?", "id": "Terkadang Disebut Ekuivalen Mayer-Norton." },
    { "en": "Apa Itu Prinsip Dualitas?", "id": "Konsep Pasangan Dalam Teori Rangkaian." },
    { "en": "Thevenin Dan Norton Adalah Contoh Sempurna Dualitas?", "id": "Ya, Benar." },
    { "en": "Dual Dari Tegangan?", "id": "Arus." },
    { "en": "Dual Dari Loop?", "id": "Simpul (Node)." },
    { "en": "Dual Dari Seri?", "id": "Paralel." },
    { "en": "Dual Dari Hubung Singkat?", "id": "Rangkaian Terbuka." },
    { "en": "Memahami Dualitas Membantu Dalam Hal Apa?", "id": "Memahami Hubungan Antar Teorema." },
    { "en": "Bisakah Saya Menggunakan Thevenin Di Dalam Thevenin?", "id": "Ya, Itu Adalah Teknik Penyederhanaan Bertahap." },
    { "en": "Misalnya?", "id": "Sederhanakan Bagian Kiri, Lalu Gunakan Hasilnya Untuk Menyederhanakan Kanan." },
    { "en": "Apakah Rangkaian Ekuivalen Memiliki Daya Sama?", "id": "Tidak, Daya Internalnya Jelas Berbeda." },
    { "en": "Daya Apa Yang Sama?", "id": "Hanya Daya Yang Dikirim Ke Beban." },
    { "en": "Apa Itu 'Dead Network'?", "id": "Rangkaian Dengan Semua Sumber Independen Dimatikan." },
    { "en": "Resistansi Thevenin Adalah Resistansi Dari 'Dead Network'?", "id": "Ya, Jika Tidak Ada Sumber Terkendali." },
    { "en": "Bagaimana Menghubungkan Baterai Untuk Daya Tahan Lebih Lama?", "id": "Secara Paralel." },
    { "en": "Bagaimana Menghubungkan Baterai Untuk Tegangan Lebih Tinggi?", "id": "Secara Seri." },
    { "en": "Bagaimana Thevenin Dari Baterai Paralel?", "id": "Vth Sama, Rth Menjadi Setengahnya." },
    { "en": "Bagaimana Thevenin Dari Baterai Seri?", "id": "Vth Menjadi Dua Kali, Rth Juga Dua Kali." },
    { "en": "Apa Yang Terjadi Jika Menghubungkan Baterai Berbeda Paralel?", "id": "Baterai Tegangan Tinggi Akan Mengisi Yang Rendah." },
    { "en": "Ini Berbahaya?", "id": "Ya, Bisa Menyebabkan Arus Besar Dan Kerusakan." },
    { "en": "Apa Itu 'Source Follower'?", "id": "Konfigurasi Penguat FET (Mirip Emitter Follower)." },
    { "en": "Resistansi Outputnya Bagaimana?", "id": "Rendah." },
    { "en": "Bagaimana Menemukan Resistansi Outputnya?", "id": "Menggunakan Analisis Thevenin." },
    { "en": "Thevenin Adalah Teorema Analisis Atau Sintesis?", "id": "Analisis (Menganalisis Rangkaian Yang Ada)." },
    { "en": "Apa Itu Sintesis Rangkaian?", "id": "Merancang Rangkaian Untuk Memenuhi Spesifikasi." },
    { "en": "Apakah Thevenin/Norton Bisa Diterapkan Pada Sistem Non-Listrik?", "id": "Ya, Dengan Menggunakan Analogi." },
    { "en": "Contoh Sistem Hidrolik?", "id": "Tekanan (Tegangan), Aliran (Arus), Hambatan Pipa (Resistansi)." },
    { "en": "Apa Itu 'Stiffness' Dalam Mekanika?", "id": "Analogi Dari Kebalikan Kapasitansi." },
    { "en": "Apa Itu 'Damping'?", "id": "Analogi Dari Resistansi." },
    { "en": "Apa Itu Massa?", "id": "Analogi Dari Induktansi." },
    { "en": "Konsep Ekuivalensi Ini Sangat Kuat?", "id": "Ya, Mendasari Banyak Pemodelan Sistem." },
    { "en": "Apa Kesalahan Umum Mahasiswa Saat Mencari Rth?", "id": "Lupa Mematikan Sumber Independen." },
    { "en": "Kesalahan Umum Lainnya?", "id": "Salah Mematikan Sumber (Tegangan Dibuka, Arus Disingkat)." },
    { "en": "Kesalahan Paling Umum Dengan Sumber Dependen?", "id": "Mematikannya Seperti Sumber Independen." },
    { "en": "Apa Itu Jaringan Dua Gerbang (Two-Port Network)?", "id": "Rangkaian Dengan Input Port Dan Output Port." },
    { "en": "Parameter Apa Yang Digunakan Untuk Menggambarkannya?", "id": "Parameter Z, Y, H, T." },
    { "en": "Apakah Parameter Ini Terkait Dengan Thevenin?", "id": "Ya, Z11 Adalah Impedansi Thevenin Di Input." },
    { "en": "Apa Itu 'Current Mirror'?", "id": "Sirkuit Yang Menyalin Arus." },
    { "en": "Resistansi Output 'Current Mirror' Ideal?", "id": "Tak Terhingga (Sumber Arus Sempurna)." },
    { "en": "Ini Adalah Resistansi Norton Atau Thevenin?", "id": "Resistansi Norton (Rn)." },
    { "en": "Jadi Ekuivalen Nortonnya Adalah?", "id": "Sumber Arus In Paralel Dengan Rn Tak Hingga." },
    { "en": "Apa Itu 'Cascode Amplifier'?", "id": "Konfigurasi Penguat Untuk Meningkatkan Kinerja." },
    { "en": "Apa Yang Ditingkatkan?", "id": "Bandwidth Dan Resistansi Output." },
    { "en": "Resistansi Output Cascode Sangat Tinggi?", "id": "Ya." },
    { "en": "Apakah Thevenin Dipakai Untuk Menganalisisnya?", "id": "Ya, Untuk Menemukan Resistansi Output Yang Tinggi." },
    { "en": "Apa Itu 'Noise Figure'?", "id": "Ukuran Degradasi Sinyal-Noise Oleh Rangkaian." },
    { "en": "Apakah Resistansi Thevenin Menghasilkan Derau?", "id": "Ya, Derau Termal (Johnson Noise)." },
    { "en": "Derau Termal Sebanding Dengan Apa?", "id": "Akar Dari Resistansi Dan Suhu." },
    { "en": "Jadi, Rth Penting Dalam Analisis Derau?", "id": "Sangat Penting." },
    { "en": "Untuk Mendapatkan Derau Rendah, Rth Sebaiknya?", "id": "Sekecil Mungkin." },
    { "en": "Apakah Teorema Thevenin Bisa Gagal?", "id": "Tidak, Selama Syarat Linearitas Terpenuhi." },
    { "en": "Jika Perhitungan Salah, Masalahnya Di Mana?", "id": "Pada Penerapan Aturan, Bukan Teoremanya." },
    { "en": "Apa Itu 'Linear Time-Invariant' (LTI) System?", "id": "Sistem Linear Yang Sifatnya Tidak Berubah Waktu." },
    { "en": "Rangkaian Thevenin Adalah Model LTI?", "id": "Ya, Benar." },
    { "en": "Apakah Thevenin Berguna Untuk Analisis Garis Transmisi?", "id": "Ya, Untuk Menganalisis Refleksi Dan Pencocokan." },
    { "en": "Impedansi Karakteristik Jalur Adalah Thevenin?", "id": "Ya, Resistansi Thevenin Dilihat Ke Jalur Tak Hingga." },
    { "en": "Apa Itu Smith Chart?", "id": "Alat Grafis Untuk Analisis Garis Transmisi." },
    { "en": "Apakah Konsep Thevenin Terkandung Di Dalamnya?", "id": "Ya, Secara Implisit Dalam Perhitungan Impedansi." },
    { "en": "Jadi, Konsep Thevenin Sangat Mendasar?", "id": "Ya, Salah Satu Konsep Paling Fundamental." },
    { "en": "Kapan Harus Menggunakan Norton Daripada Thevenin?", "id": "Ketika Lebih Tertarik Pada Arus Beban." },
    { "en": "Atau Ketika Rangkaian Sumber Lebih Mirip Sumber Arus?", "id": "Ya, Seperti Rangkaian Dengan Transistor." },
    { "en": "Apakah Hasilnya Akan Selalu Konsisten?", "id": "Ya, Thevenin Dan Norton Selalu Konsisten." },
    { "en": "Ringkasan: Thevenin Menggunakan Sumber Apa?", "id": "Sumber Tegangan Seri Resistor." },
    { "en": "Ringkasan: Norton Menggunakan Sumber Apa?", "id": "Sumber Arus Paralel Resistor." },
    { "en": "Ringkasan: Vth Ditemukan Dengan Cara Apa?", "id": "Mengukur Tegangan Terminal Terbuka (Voc)." },
    { "en": "Ringkasan: In Ditemukan Dengan Cara Apa?", "id": "Mengukur Arus Terminal Hubung Singkat (Isc)." },
    { "en": "Ringkasan: Rth Dan Rn Ditemukan Dengan Cara Apa?", "id": "Mematikan Sumber Independen Dan Mencari Resistansi." },
    { "en": "Ringkasan: Bagaimana Hubungan Rth Dan Rn?", "id": "Selalu Sama (Rth = Rn)." },
    { "en": "Ringkasan: Bagaimana Hubungan Vth, In, Rth?", "id": "Vth = In * Rth." },
    { "en": "Ringkasan: Kapan Daya Maksimum Ditransfer?", "id": "Ketika Resistansi Beban Sama Dengan Rth." },
    { "en": "Ringkasan: Syarat Untuk Rangkaian AC?", "id": "Impedansi Beban Adalah Konjugat Kompleks Zth." },
    { "en": "Ringkasan: Bagaimana Jika Ada Sumber Terkendali?", "id": "Gunakan Sumber Uji Atau Metode Voc/Isc." },
    { "en": "Apa Itu 'Bilateral Element'?", "id": "Elemen Yang Bekerja Sama Di Kedua Arah." },
    { "en": "Contoh 'Bilateral Element'?", "id": "Resistor, Induktor, Kapasitor." },
    { "en": "Apa Itu 'Unilateral Element'?", "id": "Elemen Yang Bekerja Hanya Di Satu Arah." },
    { "en": "Contoh 'Unilateral Element'?", "id": "Dioda, Transistor, Op-Amp." },
    { "en": "Teorema Thevenin Membutuhkan Rangkaian Bilateral?", "id": "Ya, Untuk Metode Pencarian Rth Sederhana." },
    { "en": "Jika Ada Komponen Unilateral, Dianggap Apa?", "id": "Sebagai Sumber Terkendali Dalam Modelnya." },
    { "en": "Jadi Teorema Tetap Berlaku?", "id": "Ya, Tapi Perlu Metode Sumber Uji." },
    { "en": "Apa Itu 'Kirchhoff's Voltage Law' (KVL)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Adalah Nol." },
    { "en": "Apa Itu 'Kirchhoff's Current Law' (KCL)?", "id": "Jumlah Arus Di Simpul Adalah Nol." },
    { "en": "Apakah Thevenin/Norton Menggantikan Hukum Kirchhoff?", "id": "Tidak, Mereka Turunan Dari Hukum Kirchhoff." },
    { "en": "Mereka Adalah Jalan Pintas?", "id": "Ya, Jalan Pintas Untuk Masalah Tertentu." },
    { "en": "Apa Itu 'Linear Superposition'?", "id": "Prinsip Superposisi." },
    { "en": "Apakah Superposisi Bisa Untuk Mencari Rth?", "id": "Tidak, Rth Adalah Sifat Rangkaian Pasif." },
    { "en": "Superposisi Digunakan Untuk Mencari Apa?", "id": "Tegangan Dan Arus (Vth Atau Isc)." },
    { "en": "Langkah Superposisi?", "id": "Analisis Efek Setiap Sumber Satu Per Satu." },
    { "en": "Apakah Thevenin Selalu Lebih Cepat Dari Superposisi?", "id": "Tidak Selalu, Tergantung Rangkaiannya." },
    { "en": "Apa Itu 'Parameter Hibrida' (h-parameters)?", "id": "Salah Satu Set Parameter Jaringan Dua Port." },
    { "en": "Parameter Ini Terkait Thevenin?", "id": "Ya, h22 Adalah Admitansi Thevenin Di Output." },
    { "en": "Apa Itu 'Open-Circuit Voltage Gain'?", "id": "Penguatan Tegangan Tanpa Beban." },
    { "en": "Ini Sama Dengan Apa?", "id": "Rasio Vth Terhadap Tegangan Input." },
    { "en": "Apa Kata Kunci Untuk Thevenin?", "id": "Tegangan, Terbuka, Seri." },
    { "en": "Apa Kata Kunci Untuk Norton?", "id": "Arus, Singkat, Paralel." },
    { "en": "Apa Kesamaan Fundamental Keduanya?", "id": "Keduanya Menggunakan Resistansi Internal Yang Sama." },
    { "en": "Manakah Yang Lebih Intuitif?", "id": "Bagi Banyak Orang, Thevenin Lebih Intuitif." },
    { "en": "Mengapa Thevenin Lebih Intuitif?", "id": "Karena Mirip Dengan Model Baterai Nyata." },
    { "en": "Apakah Thevenin Bisa Diterapkan Di Frekuensi Sangat Tinggi?", "id": "Ya, Tapi Efek Garis Transmisi Menjadi Penting." },
    { "en": "Apa Itu 'Scattering Parameters' (S-parameters)?", "id": "Parameter Untuk Menganalisis Jaringan Frekuensi Tinggi." },
    { "en": "Apakah S-parameters Terkait Thevenin?", "id": "Ya, Bisa Dikonversi Ke Z-parameters (Dan Thevenin)." },
    { "en": "Apa Itu 'SPICE'?", "id": "Program Simulasi Rangkaian Listrik." },
    { "en": "Apakah SPICE Bisa Menghitung Ekuivalen Thevenin?", "id": "Ya, Dengan Perintah Khusus." },
    { "en": "Bagaimana SPICE Melakukannya?", "id": "Secara Otomatis Menghitung Voc Dan Isc." },
    { "en": "Apakah Teorema Ini Penting Untuk Desain IC?", "id": "Ya, Untuk Memodelkan Blok-blok Fungsional." },
    { "en": "Setiap Blok Dapat Direpresentasikan Sebagai Apa?", "id": "Ekuivalen Thevenin Di Outputnya." },
    { "en": "Apa Itu 'Equivalent Series Resistance' (ESR) Kapasitor?", "id": "Resistansi Thevenin Internal Kapasitor Nyata." },
    { "en": "ESR Rendah Itu Baik Atau Buruk?", "id": "Baik, Menandakan Kapasitor Berkualitas Tinggi." },
    { "en": "Bagaimana Thevenin Dari Beberapa Sumber Paralel?", "id": "Gunakan Teorema Millman Untuk Menyederhanakannya." },
    { "en": "Teorema Millman Memberikan Apa?", "id": "Tegangan Ekuivalen Tunggal (Vth)." },
    { "en": "Apa Itu 'Compensation Theorem'?", "id": "Teorema Tentang Efek Perubahan Impedansi." },
    { "en": "Apakah Terkait Thevenin?", "id": "Ya, Bisa Digunakan Untuk Menganalisis Perubahan." },
    { "en": "Bisakah Saya Mengabaikan Sumber Dependen?", "id": "Tidak, Itu Akan Memberikan Jawaban Salah." },
    { "en": "Apa Hal Paling Penting Untuk Diingat?", "id": "Bedakan Antara Sumber Independen Dan Dependen." },
    { "en": "Langkah Verifikasi Yang Baik?", "id": "Pastikan Satuan Sudah Benar (Volt, Ampere, Ohm)." },
    { "en": "Jika Saya Menemukan Vth, Bisakah Saya Langsung Menemukan In?", "id": "Ya, Jika Rth Sudah Diketahui." },
    { "en": "Jadi Hanya Perlu Menghitung Dua Dari Tiga Parameter?", "id": "Ya, Benar." },
    { "en": "Metode Mana Yang Paling Andal?", "id": "Semua Metode Andal Jika Diterapkan Dengan Benar." },
    { "en": "Pilihan Metode Mempengaruhi Jawaban Akhir?", "id": "Tidak, Hanya Mempengaruhi Jalan Menuju Jawaban." },
    { "en": "Apakah Thevenin Mengubah Rangkaian Secara Fisik?", "id": "Tidak, Ini Adalah Alat Matematis." },
    { "en": "Apakah Model Thevenin Memprediksi Arus Di Dalam Sumber?", "id": "Tidak, Hanya Di Luar Terminal." },
    { "en": "Apa Tujuan Utama Belajar Teorema Ini?", "id": "Mengembangkan Intuisi Tentang Perilaku Rangkaian." },
    { "en": "Intuisi Tentang Apa?", "id": "Tentang Konsep Resistansi Internal Dan Pembebanan." },
    { "en": "Jika Sebuah Speaker 8 Ohm Dihubungkan Ke Amplifier 8 Ohm?", "id": "Terjadi Transfer Daya Maksimum." },
    { "en": "Ini Baik Untuk Kualitas Suara?", "id": "Ya, Dan Untuk Keamanan Amplifier." },
    { "en": "Apa Itu 'Damping Factor'?", "id": "Rasio Impedansi Beban Terhadap Impedansi Output." },
    { "en": "'Damping Factor' Tinggi Artinya Apa?", "id": "Impedansi Output Amplifier Sangat Rendah." },
    { "en": "Ini Baik Untuk Mengontrol Speaker?", "id": "Ya, Sangat Baik." },
    { "en": "Impedansi Output Amplifier Adalah Rth-nya?", "id": "Ya, Betul." },
    { "en": "Apakah Mungkin Rangkaian Tidak Memiliki Ekuivalen Thevenin?", "id": "Sangat Jarang, Mungkin Jika Non-Linear Atau Tidak Stabil." },
    { "en": "Apa Itu 'Node'?", "id": "Titik Pertemuan Komponen." },
    { "en": "Apa Itu 'Branch'?", "id": "Jalur Antara Dua Node." },
    { "en": "Apa Itu 'Loop'?", "id": "Jalur Tertutup Dalam Rangkaian." },
    { "en": "Konsep-konsep Ini Adalah Dasar Untuk Analisis Apa?", "id": "Analisis Nodal Dan Mesh." },
    { "en": "Yang Digunakan Untuk Menemukan Vth Dan Isc?", "id": "Ya, Betul." },
    { "en": "Apakah Ekuivalen Thevenin Tergantung Waktu?", "id": "Tidak, Untuk Rangkaian LTI." },
    { "en": "Bagaimana Jika Nilai Resistor Berubah?", "id": "Maka Ekuivalen Thevenin Juga Akan Berubah." },
    { "en": "Apa Intisari Dari Teorema Thevenin?", "id": "Setiap Rangkaian Linear Terlihat Seperti Baterai Nyata." },
    { "en": "Dan Intisari Dari Teorema Norton?", "id": "Setiap Rangkaian Linear Terlihat Seperti Sumber Arus Nyata." },
    { "en": "Keduanya Adalah Pernyataan Ekuivalensi Yang Kuat?", "id": "Ya, Sangat Kuat Dan Berguna." },
    { "en": "Apa Cara Terbaik Untuk Belajar?", "id": "Mengerjakan Banyak Soal Latihan." },
    { "en": "Mulai Dari Rangkaian Sederhana?", "id": "Ya, Lalu Tingkatkan Kesulitannya." },
    { "en": "Hati-hati Dengan Tanda Aljabar?", "id": "Ya, Terutama Dalam Analisis Nodal/Mesh." },
    { "en": "Dan Dengan Perhitungan Bilangan Kompleks?", "id": "Ya, Untuk Rangkaian AC." },
    { "en": "Teorema Ini Adalah Keterampilan Wajib Insinyur Elektro?", "id": "Ya, Salah Satu Keterampilan Paling Dasar." }




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
