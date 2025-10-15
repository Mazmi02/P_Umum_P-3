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


  { "en": "Apa Nama Ibukota Negara Uganda?", "id": "Kampala." },
  { "en": "Siapa Penulis Novel Fiksi 'Invisible Man'?", "id": "Ralph Ellison." },
  { "en": "Apa Sebutan Untuk Studi Tentang Aves?", "id": "Ornitologi." },
  { "en": "Berapa Jumlah Muka Sisi Pada Sebuah Piramida Segilima?", "id": "6 Muka." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Skoda?", "id": "Ceko." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Ar?", "id": "Argon." },
  { "en": "Siapakah Yang Menulis Buku 'Meditations'?", "id": "Marcus Aurelius." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Anggur?", "id": "Buah Anggur." },
  { "en": "Kapan Peringatan Hari Bahasa Inggris Sedunia?", "id": "23 April." },
  { "en": "Apa Nama Proses Penguraian Air Oleh Cahaya?", "id": "Fotolisis." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Perancis?", "id": "Marie Antoinette." },
  { "en": "Apa Nama Gerakan Memutar Badan Dalam Balet?", "id": "Pirouette." },
  { "en": "Apa Nama Mata Uang Resmi Negara Finlandia?", "id": "Euro." },
  { "en": "Dari Benua Manakah Asal Tanaman Karet?", "id": "Amerika Selatan." },
  { "en": "Proses Pengerasan Sedimen Menjadi Batuan Disebut?", "id": "Litifikasi." },
  { "en": "Siapa Penulis Novel 'The Great Gatsby'?", "id": "F. Scott Fitzgerald." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Pineal?", "id": "Melatonin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Wafat Isa Almasih?", "id": "Kristen." },
  { "en": "Apa Sebutan Untuk Bintang Terdekat Dengan Matahari?", "id": "Proxima Centauri." },
  { "en": "Berapa Jumlah Tulang Rusuk Melayang Pada Manusia?", "id": "4 Tulang (2 Pasang)." },
  { "en": "Siapakah Pendiri Dinasti Joseon Di Korea?", "id": "Yi Seong-gye." },
  { "en": "Apa Akronim Untuk Basic Input/Output System?", "id": "BIOS." },
  { "en": "Di Kota Manakah Terdapat Jembatan Ponte Vecchio?", "id": "Florence, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berubah Warna?", "id": "Bunglon." },
  { "en": "Apa Nama Tarian Tradisional Dari Jawa Timur?", "id": "Tari Reog Ponorogo." },
  { "en": "Siapa Nama Dewi Kebijaksanaan Dalam Mitologi Yunani?", "id": "Athena." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara Brasil?", "id": "26 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Inti Bumi Yang Padat?", "id": "Inti Dalam." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Praha?", "id": "Ceko." },
  { "en": "Apa Nama Senjata Tradisional Dari Suku Madura?", "id": "Celurit." },
  { "en": "Siapakah Yang Mengemukakan Teori Kuantum Medan?", "id": "Paul Dirac." },
  { "en": "Apa Nama Skala Untuk Mengukur Kekerasan Mineral?", "id": "Skala Mohs." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Sirilik?", "id": "33 Huruf." },
  { "en": "Jaringan Apa Yang Mengangkut Hasil Fotosintesis?", "id": "Floem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Seribu Danau?", "id": "Finlandia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Dadaisme?", "id": "Tristan Tzara." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Las Listrik?", "id": "Argon." },
  { "en": "Apa Nama Laut Yang Memisahkan Afrika Dan Asia?", "id": "Laut Merah." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Anatomi?", "id": "Herophilos." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Imun?", "id": "Imunologi." },
  { "en": "Berapa Jarak Lari Gawang Untuk Wanita?", "id": "100 Meter." },
  { "en": "Apa Nama Ibukota Negara Siprus?", "id": "Nikosia." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel '1984'?", "id": "Winston Smith." },
  { "en": "Kekurangan Mangan Dapat Menyebabkan Masalah Apa?", "id": "Gangguan Tulang." },
  { "en": "Apa Nama Katedral Terkenal Di Kota Moskow?", "id": "Katedral Santo Basil." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Hipofisis?", "id": "Kelenjar Pituitari." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Kota Chandigarh?", "id": "Le Corbusier." },
  { "en": "Apa Istilah Untuk Pukulan Keras Dalam Voli?", "id": "Spike." },
  { "en": "Dimana Lokasi Tembok Besar Tiongkok Berakhir?", "id": "Gurun Gobi." },
  { "en": "Apa Sebutan Untuk Studi Tentang Lumut Kerak?", "id": "Likenologi." },
  { "en": "Siapa Pemimpin Amerika Serikat Saat Perang Teluk?", "id": "George H. W. Bush." },
  { "en": "Apa Nama Gurun Terluas Di Amerika Selatan?", "id": "Gurun Patagonia." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segisepuluh?", "id": "12 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Penuaan?", "id": "Gerontologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kertas?", "id": "Tiongkok." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Kalingga?", "id": "Ratu Shima." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Proses Erosi?", "id": "Batuan Sedimen." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Kaukasus?", "id": "Gunung Elbrus." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Swift?", "id": "Chris Lattner." },
  { "en": "Apa Nama Pakaian Tradisional Pria Vietnam?", "id": "Ao Dai." },
  { "en": "Huruf Apa Yang Melambangkan Angka 50 Dalam Romawi?", "id": "L." },
  { "en": "Apa Satuan Standar Internasional Untuk Tekanan?", "id": "Pascal." },
  { "en": "Siapakah Kaisar Romawi Yang Memerintah Paling Singkat?", "id": "Gordian I." },
  { "en": "Apa Nama Hidangan Daging Asap Khas Brasil?", "id": "Churrasco." },
  { "en": "Apa Nama Hewan Darat Paling Cerdas Di Asia?", "id": "Gajah Asia." },
  { "en": "Apa Akronim Untuk Bank Pembangunan Antar-Amerika?", "id": "IDB (Inter-American Development Bank)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segisepuluh?", "id": "12 Bidang." },
  { "en": "Apa Nama Ibukota Negara Azerbaijan?", "id": "Baku." },
  { "en": "Siapa Penulis Novel 'The Handmaid's Tale'?", "id": "Margaret Atwood." },
  { "en": "Apa Istilah Untuk Studi Tentang Kanker?", "id": "Onkologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional China?", "id": "Panda." },
  { "en": "Apa Ibukota Provinsi Papua Pegunungan?", "id": "Jayawijaya." },
  { "en": "Siapa Nama Dewa Pemelihara Dalam Mitologi Hindu?", "id": "Wisnu." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Batubara?", "id": "Karbon." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Citizens'?", "id": "Manchester City." },
  { "en": "Apa Sebutan Untuk Bintang Yang Telah Mati?", "id": "Bintang Katai Putih." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Pajang?", "id": "Sultan Hadiwijaya." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Filologi?", "id": "Filolog." },
  { "en": "Dimana Lokasi Air Terjun Angel Berada?", "id": "Venezuela." },
  { "en": "Berapa Jumlah Tulang Belakang Pinggang Pada Manusia?", "id": "5 Tulang." },
  { "en": "Apa Istilah Untuk Angin Yang Berhembus Dari Lembah?", "id": "Angin Lembah." },
  { "en": "Siapakah Bapak Psikologi Kognitif?", "id": "Ulric Neisser." },
  { "en": "Apa Sebutan Untuk Proses Pembelahan Inti Atom?", "id": "Fisi Nuklir." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Ultimate Frisbee?", "id": "7 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Angin?", "id": "Anemometer." },
  { "en": "Siapakah Jenderal Athena Yang Terkenal Dengan Pidato Pemakamannya?", "id": "Pericles." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Orang Tua?", "id": "Gerontokrasi." },
  { "en": "Apa Akronim Untuk 'In Other Words'?", "id": "IOW." },
  { "en": "Pada Tahun Berapa Revolusi Perancis Berakhir?", "id": "Tahun 1799." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Berjalannya?", "id": "Alberto Giacometti." },
  { "en": "Apa Nama Ibukota Negara Latvia?", "id": "Riga." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Yunani?", "id": "2 Warna." },
  { "en": "Apa Proses Pembentukan Delta Di Danau?", "id": "Deposisi Sedimen." },
  { "en": "Siapa Penjelajah Yang Menemukan Jalur Laut Ke India?", "id": "Vasco da Gama." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Eropa?", "id": "Gunung Etna." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Epinefrin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putra Mahkota Astina'?", "id": "Abimanyu." },
  { "en": "Di Negara Mana Festival Musik Lollapalooza Diadakan?", "id": "Amerika Serikat." },
  { "en": "Apa Nama Ibukota Negara Chad?", "id": "N'Djamena." },
  { "en": "Siapa Penulis Novel Fiksi Gotik 'Frankenstein'?", "id": "Mary Shelley." },
  { "en": "Apa Sebutan Untuk Studi Tentang Proses Penuaan?", "id": "Gerontologi." },
  { "en": "Berapa Jumlah Muka Sisi Pada Sebuah Kubus?", "id": "6 Muka." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Pagani?", "id": "Italia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Xe?", "id": "Xenon." },
  { "en": "Siapakah Yang Menulis Buku 'The Interpretation of Dreams'?", "id": "Sigmund Freud." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Anggur?", "id": "Buah Anggur." },
  { "en": "Kapan Peringatan Hari Solidaritas Manusia Internasional?", "id": "20 Desember." },
  { "en": "Apa Nama Proses Pergerakan Air Melalui Akar?", "id": "Osmosis." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Inggris?", "id": "Ratu Elizabeth II." },
  { "en": "Apa Istilah Untuk Gerakan Meluncur Di Atas Papan?", "id": "Skateboarding." },
  { "en": "Apa Nama Mata Uang Resmi Negara Maroko?", "id": "Dirham Maroko." },
  { "en": "Dari Benua Manakah Asal Tanaman Kopi?", "id": "Afrika." },
  { "en": "Proses Perubahan Batuan Oleh Air Panas Disebut?", "id": "Alterasi Hidrotermal." },
  { "en": "Siapa Penulis Novel 'One Flew Over the Cuckoo's Nest'?", "id": "Ken Kesey." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Kortisol." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Wafat Isa Almasih?", "id": "Kristen." },
  { "en": "Apa Sebutan Untuk Awan Yang Menghasilkan Hujan?", "id": "Awan Nimbus." },
  { "en": "Berapa Jumlah Tulang Jari Tangan Pada Manusia?", "id": "14 Tulang." },
  { "en": "Siapakah Pendiri Dinasti Ming Di Tiongkok?", "id": "Zhu Yuanzhang." },
  { "en": "Apa Akronim Untuk Organisasi Riset Nuklir Eropa?", "id": "CERN." },
  { "en": "Di Kota Manakah Terdapat Museum Vatikan?", "id": "Kota Vatikan." },
  { "en": "Hewan Apa Yang Dikenal Dapat Tidur Sambil Berdiri?", "id": "Kuda." },
  { "en": "Apa Nama Upacara Adat Pemakaman Di Toraja?", "id": "Rambu Solo'." },
  { "en": "Siapa Nama Dewi Pelindung Pernikahan Romawi?", "id": "Juno." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara Australia?", "id": "6 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Terluar Planet Bumi?", "id": "Kerak Bumi." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Oslo?", "id": "Norwegia." },
  { "en": "Apa Nama Senjata Tradisional Dari Suku Dayak?", "id": "Mandau." },
  { "en": "Siapakah Yang Mengemukakan Hukum Termodinamika?", "id": "Sadi Carnot." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Warna?", "id": "Korona." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Korea?", "id": "24 Huruf." },
  { "en": "Jaringan Apa Yang Mengangkut Air Dan Nutrisi?", "id": "Jaringan Vaskular." },
  { "en": "Negara Apa Yang Dijuluki Negeri Seribu Danau?", "id": "Finlandia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Kubisme?", "id": "Pablo Picasso." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Proses Pengelasan?", "id": "Asetilena." },
  { "en": "Apa Nama Selat Yang Memisahkan Asia Dan Amerika?", "id": "Selat Bering." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Sosiologi?", "id": "Auguste Comte." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Pencernaan?", "id": "Gastroenterologi." },
  { "en": "Berapa Jarak Lari Maraton Penuh?", "id": "42.195 Meter." },
  { "en": "Apa Nama Ibukota Negara Jamaika?", "id": "Kingston." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Don Quixote'?", "id": "Alonso Quijano." },
  { "en": "Kekurangan Kalium Dapat Menyebabkan Masalah Apa?", "id": "Kelemahan Otot." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Madrid?", "id": "Istana Kerajaan Madrid." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Keringat?", "id": "Kulit." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Centre Pompidou?", "id": "Renzo Piano." },
  { "en": "Apa Istilah Untuk Serangan Cepat Dalam Sepak Bola?", "id": "Serangan Balik." },
  { "en": "Dimana Lokasi Angkor Wat, Kompleks Candi Megah?", "id": "Siem Reap, Kamboja." },
  { "en": "Apa Sebutan Untuk Studi Tentang Jamur Beracun?", "id": "Mikotoksikologi." },
  { "en": "Siapa Pemimpin Gerakan Reformasi Protestan?", "id": "Martin Luther." },
  { "en": "Apa Nama Gurun Terpanas Di Dunia?", "id": "Gurun Lut." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Prisma Segienam?", "id": "12 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Gunung?", "id": "Orologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kopi Espresso?", "id": "Italia." },
  { "en": "Siapakah Ratu Yang Memerintah Spanyol Saat Era Columbus?", "id": "Ratu Isabella I." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Pendinginan Cepat Lava?", "id": "Batuan Beku Vulkanik." },
  { "en": "Apa Nama Titik Tertinggi Di Selandia Baru?", "id": "Gunung Cook." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman C?", "id": "Dennis Ritchie." },
  { "en": "Apa Nama Kain Tenun Khas Dari Suku Batak?", "id": "Ulos." },
  { "en": "Huruf Apa Yang Melambangkan Angka 100 Dalam Romawi?", "id": "C." },
  { "en": "Apa Satuan Standar Internasional Untuk Fluks Magnetik?", "id": "Weber." },
  { "en": "Siapakah Kaisar Romawi Yang Memerintah Paling Lama?", "id": "Augustus." },
  { "en": "Apa Nama Hidangan Nasi Khas Dari Jepang?", "id": "Sushi." },
  { "en": "Apa Nama Hewan Darat Paling Berbisa Di Australia?", "id": "Ular Taipan Pedalaman." },
  { "en": "Apa Akronim Untuk Deklarasi Universal Hak Asasi Manusia?", "id": "UDHR." },
  { "en": "Berapa Jumlah Bidang Diagonal Pada Sebuah Balok?", "id": "6 Bidang Diagonal." },
  { "en": "Apa Nama Ibukota Negara Estonia?", "id": "Tallinn." },
  { "en": "Siapa Penulis Novel 'The Stranger'?", "id": "Albert Camus." },
  { "en": "Apa Istilah Untuk Studi Tentang Moluska?", "id": "Malakologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Indonesia?", "id": "Elang Jawa." },
  { "en": "Apa Ibukota Provinsi Jawa Tengah?", "id": "Semarang." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Mesir?", "id": "Sekhmet." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Tulang?", "id": "Kalsium Fosfat." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Blues'?", "id": "Chelsea FC." },
  { "en": "Apa Sebutan Untuk Titik Terdekat Planet Ke Bintangnya?", "id": "Periastron." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Sriwijaya?", "id": "Balaputradewa." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Filologi?", "id": "Filolog." },
  { "en": "Dimana Lokasi Gunung Fuji Yang Simetris?", "id": "Pulau Honshu, Jepang." },
  { "en": "Berapa Jumlah Tulang Metatarsal Pada Kaki Manusia?", "id": "5 Tulang." },
  { "en": "Apa Istilah Untuk Gerakan Udara Dari Tekanan Tinggi?", "id": "Angin." },
  { "en": "Siapakah Bapak Taksonomi Hewan Yang Terkenal?", "id": "Carolus Linnaeus." },
  { "en": "Apa Sebutan Untuk Proses Pembekuan Air?", "id": "Pembekuan." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Rugby League?", "id": "13 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Rotasi?", "id": "Takometer." },
  { "en": "Siapakah Kaisar Romawi Yang Membangun Tembok Hadrian?", "id": "Hadrian." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Bangsawan Gereja?", "id": "Episkopal." },
  { "en": "Apa Akronim Untuk 'As Soon As Possible'?", "id": "ASAP." },
  { "en": "Pada Tahun Berapa Perang Dingin Secara Resmi Berakhir?", "id": "Tahun 1991." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Berjalannya?", "id": "Alberto Giacometti." },
  { "en": "Apa Nama Ibukota Negara Siprus?", "id": "Nikosia." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Kanada?", "id": "2 Warna." },
  { "en": "Apa Proses Pelepasan Uap Air Dari Daun?", "id": "Transpirasi." },
  { "en": "Siapa Penjelajah Yang Mencapai Puncak Gunung Everest Pertama?", "id": "Sir Edmund Hillary." },
  { "en": "Apa Nama Palung Terdalam Di Samudra Pasifik?", "id": "Palung Mariana." },
  { "en": "Apa Hormon Yang Mengatur Tingkat Gula Darah?", "id": "Insulin Dan Glukagon." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Kesatria Pringgandani'?", "id": "Gatotkaca." },
  { "en": "Di Negara Mana Festival Lentera Musim Dingin Dirayakan?", "id": "Tiongkok." },
  { "en": "Apa Nama Ibukota Negara Komoro?", "id": "Moroni." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1982?", "id": "Gabriel García Márquez." },
  { "en": "Apa Simbol Kimia Untuk Unsur Antimon?", "id": "Sb (Stibium)." },
  { "en": "Siapa Pahlawan Nasional Indonesia Dari Papua?", "id": "Silas Papare." },
  { "en": "Apa Istilah Untuk Batu Dalam Olahraga Curling?", "id": "Stone." },
  { "en": "Siapa Arsitek Yang Merancang Gedung The Shard?", "id": "Renzo Piano." },
  { "en": "Siapa Nama Dewi Malam Dalam Mitologi Yunani?", "id": "Nyx." },
  { "en": "Apa Tujuan Utama Dari Perjanjian Tordesillas?", "id": "Membagi Dunia Baru." },
  { "en": "Siapakah Yang Dianggap Sebagai 'Nenek COBOL'?", "id": "Grace Hopper." },
  { "en": "Organel Sel Yang Berfungsi Sebagai 'Pembangkit Tenaga'?", "id": "Mitokondria." },
  { "en": "Apa Nama Ibukota Negara Saint Lucia?", "id": "Castries." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1993?", "id": "Nelson Mandela, F. W. de Klerk." },
  { "en": "Apa Simbol Kimia Untuk Unsur Bismut?", "id": "Bi." },
  { "en": "Siapa Pahlawan Nasional Indonesia Dari Bali?", "id": "I Gusti Ngurah Rai." },
  { "en": "Apa Istilah Untuk Sapuan Dalam Olahraga Curling?", "id": "Sweeping." },
  { "en": "Siapa Arsitek Utama Yang Merancang Taipei 101?", "id": "C.Y. Lee." },
  { "en": "Siapa Nama Dewa Penipuan Dalam Mitologi Nordik?", "id": "Loki." },
  { "en": "Kapan Perjanjian Versailles Secara Resmi Ditandatangani?", "id": "Tahun 1919." },
  { "en": "Siapa Yang Dikenal Sebagai Bapak Kecerdasan Buatan?", "id": "John McCarthy." },
  { "en": "Organel Sel Yang Berfungsi Mensintesis Protein?", "id": "Ribosom." },
  { "en": "Apa Nama Ibukota Negara Sao Tome Dan Principe?", "id": "São Tomé." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1921?", "id": "Albert Einstein." },
  { "en": "Apa Simbol Kimia Untuk Unsur Teknesium?", "id": "Tc." },
  { "en": "Siapa Pahlawan Nasional Indonesia Dari Sulawesi Utara?", "id": "Wolter Monginsidi." },
  { "en": "Apa Nama Gerakan Menyerang Dalam Olahraga Anggar?", "id": "Lunge." },
  { "en": "Siapa Arsitek Yang Merancang Museum Guggenheim Bilbao?", "id": "Frank Gehry." },
  { "en": "Siapa Nama Dewa Kematian Dalam Mitologi Mesir?", "id": "Anubis." },
  { "en": "Apa Isi Utama Dari Piagam Magna Carta?", "id": "Membatasi Kekuasaan Raja." },
  { "en": "Siapa Yang Menciptakan Bahasa Pemrograman Pascal?", "id": "Niklaus Wirth." },
  { "en": "Apa Nama Proses Pembelahan Sel Somatik?", "id": "Mitosis." },
  { "en": "Apa Nama Ibukota Negara Kiribati?", "id": "Tarawa Selatan." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1911?", "id": "Marie Curie." },
  { "en": "Apa Simbol Kimia Untuk Unsur Astatin?", "id": "At." },
  { "en": "Siapa Pahlawan Nasional Wanita Dari Sulawesi Selatan?", "id": "Opu Daeng Risaju." },
  { "en": "Apa Nama Area Target Dalam Olahraga Panahan?", "id": "Face." },
  { "en": "Siapa Arsitek Terkenal Perancang Katedral Brasília?", "id": "Oscar Niemeyer." },
  { "en": "Siapa Nama Dewi Pelangi Dalam Mitologi Nordik?", "id": "Bifröst." },
  { "en": "Apa Tujuan Dari Proklamasi Emansipasi Di AS?", "id": "Membebaskan Para Budak." },
  { "en": "Siapa Yang Dianggap Sebagai Bapak World Wide Web?", "id": "Tim Berners-Lee." },
  { "en": "Apa Nama Cairan Di Dalam Sel?", "id": "Sitoplasma." },
  { "en": "Apa Nama Ibukota Negara Antigua Dan Barbuda?", "id": "St. John's." },
  { "en": "Siapa Pemenang Nobel Fisiologi Tahun 1962?", "id": "Watson, Crick, Wilkins." },
  { "en": "Apa Simbol Kimia Untuk Unsur Protaktinium?", "id": "Pa." },
  { "en": "Siapa Pahlawan Nasional Indonesia Dari Kalimantan Selatan?", "id": "Pangeran Antasari." },
  { "en": "Apa Istilah Untuk Pelanggaran Dalam Anggar?", "id": "Infraction." },
  { "en": "Siapa Arsitek Yang Merancang Sydney Opera House?", "id": "Jørn Utzon." },
  { "en": "Siapa Nama Dewa Laut Dalam Mitologi Jepang?", "id": "Ryūjin." },
  { "en": "Apa Hasil Dari Konferensi Meja Bundar (KMB)?", "id": "Pengakuan Kedaulatan Indonesia." },
  { "en": "Siapa Yang Menciptakan Mouse Komputer Pertama?", "id": "Douglas Engelbart." },
  { "en": "Apa Nama Membran Yang Membungkus Sel?", "id": "Membran Sel." },
  { "en": "Apa Nama Ibukota Negara Grenada?", "id": "St. George's." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1979?", "id": "Bunda Teresa." },
  { "en": "Apa Simbol Kimia Untuk Unsur Telurium?", "id": "Te." },
  { "en": "Siapa Pahlawan Nasional Indonesia Asal Tapanuli?", "id": "Sisingamangaraja XII." },
  { "en": "Apa Sebutan Untuk Posisi Jongkok Dalam Anggar?", "id": "En Garde." },
  { "en": "Siapa Arsitek Yang Merancang Museum Louvre Pyramid?", "id": "I. M. Pei." },
  { "en": "Siapa Nama Anjing Berkepala Tiga Mitologi Yunani?", "id": "Cerberus." },
  { "en": "Apa Tujuan Utama Rencana Marshall Pasca-PD II?", "id": "Membangun Kembali Eropa." },
  { "en": "Siapa Yang Dikenal Sebagai Bapak Teori Informasi?", "id": "Claude Shannon." },
  { "en": "Apa Nama Proses Sel 'Memakan' Partikel Luar?", "id": "Fagositosis." },
  { "en": "Apa Nama Ibukota Negara Saint Kitts Dan Nevis?", "id": "Basseterre." },
  { "en": "Siapa Pemenang Nobel Ekonomi Pada Tahun 1976?", "id": "Milton Friedman." },
  { "en": "Apa Simbol Kimia Untuk Unsur Indium?", "id": "In." },
  { "en": "Siapa Pahlawan Nasional Indonesia Dari Maluku Utara?", "id": "Sultan Nuku." },
  { "en": "Apa Nama Target Tengah Dalam Panahan?", "id": "Bullseye." },
  { "en": "Siapa Arsitek Yang Merancang Fallingwater House?", "id": "Frank Lloyd Wright." },
  { "en": "Siapa Nama Makhluk Setengah Manusia Setengah Kuda?", "id": "Centaur." },
  { "en": "Apa Perjanjian Yang Mengakhiri Perang Dunia I?", "id": "Perjanjian Versailles." },
  { "en": "Siapa Yang Menciptakan Bahasa Pemrograman C++?", "id": "Bjarne Stroustrup." },
  { "en": "Apa Nama Organel Yang Mencerna Limbah Sel?", "id": "Lisosom." },
  { "en": "Apa Nama Ibukota Negara Vanuatu?", "id": "Port Vila." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1954?", "id": "Ernest Hemingway." },
  { "en": "Apa Simbol Kimia Untuk Unsur Talium?", "id": "Tl." },
  { "en": "Siapa Pahlawan Nasional Indonesia Dari Aceh?", "id": "Teuku Umar." },
  { "en": "Apa Istilah Untuk Kemenangan Dalam Berkuda?", "id": "Clear Round." },
  { "en": "Siapa Arsitek Yang Merancang Empire State Building?", "id": "Shreve, Lamb & Harmon." },
  { "en": "Siapa Nama Dewi Bulan Dalam Mitologi Jepang?", "id": "Tsukuyomi." },
  { "en": "Apa Perjanjian Yang Membeli Alaska Dari Rusia?", "id": "Pembelian Alaska." },
  { "en": "Siapa Yang Dikenal Sebagai Bapak Komputer Pribadi?", "id": "Henry Edward Roberts." },
  { "en": "Apa Nama Pusat Kendali Aktivitas Sel?", "id": "Nukleus (Inti Sel)." },
  { "en": "Apa Nama Ibukota Negara Tonga?", "id": "Nuku'alofa." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1903?", "id": "Becquerel, Pierre Dan Marie Curie." },
  { "en": "Apa Simbol Kimia Untuk Unsur Renium?", "id": "Re." },
  { "en": "Siapa Pahlawan Nasional Asal Sumatera Barat?", "id": "Tuanku Imam Bonjol." },
  { "en": "Apa Nama Lintasan Dalam Olahraga Berkuda?", "id": "Arena Atau Course." },
  { "en": "Siapa Arsitek Yang Merancang Menara Kembar Petronas?", "id": "Cesar Pelli." },
  { "en": "Siapa Nama Makhluk Mitologi Bertubuh Singa Berkepala Manusia?", "id": "Sphinx." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Korea?", "id": "Persetujuan Gencatan Senjata Korea." },
  { "en": "Siapa Penemu Sirkuit Terpadu Atau Chip Komputer?", "id": "Jack Kilby, Robert Noyce." },
  { "en": "Apa Nama Jaringan Saraf Di Dalam Sel?", "id": "Retikulum Endoplasma." },
  { "en": "Apa Nama Tanah Genting Yang Menghubungkan Afrika Dan Asia?", "id": "Tanah Genting Suez." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Dinamit?", "id": "Alfred Nobel." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Belikat Manusia?", "id": "Skapula." },
  { "en": "Apa Nama Ibukota Kuno Kerajaan Khmer?", "id": "Angkor Thom." },
  { "en": "Apa Arti Istilah Musik 'Adagio'?", "id": "Lambat." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep 'Tabula Rasa'?", "id": "John Locke." },
  { "en": "Apa Nama Ibukota Negara Kyrgyzstan?", "id": "Bishkek." },
  { "en": "Apa Nama Kesenian Wayang Boneka Kayu Dari Jawa Barat?", "id": "Wayang Golek." },
  { "en": "Siapa Penemu Hukum Gas Tentang Tekanan Dan Volume?", "id": "Robert Boyle." },
  { "en": "Apa Nama Wahana Antariksa Yang Mengunjungi Jupiter?", "id": "Juno." },
  { "en": "Semenanjung Apa Yang Ditempati Oleh Negara Korea?", "id": "Semenanjung Korea." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Revolver?", "id": "Samuel Colt." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Lutut Manusia?", "id": "Patela." },
  { "en": "Apa Nama Ibukota Kerajaan Aztec Yang Megah?", "id": "Tenochtitlan." },
  { "en": "Apa Arti Istilah Musik 'Crescendo'?", "id": "Semakin Keras." },
  { "en": "Siapa Filsuf Yang Mengatakan 'Saya Berpikir, Maka Saya Ada'?", "id": "René Descartes." },
  { "en": "Apa Nama Ibukota Negara Turkmenistan?", "id": "Ashgabat." },
  { "en": "Apa Nama Motif Batik Yang Berbentuk Seperti Pedang?", "id": "Parang." },
  { "en": "Siapa Penemu Hukum Gas Tentang Volume Dan Suhu?", "id": "Jacques Charles." },
  { "en": "Apa Nama Misi Berawak Pertama Ke Bulan?", "id": "Apollo 11." },
  { "en": "Apa Nama Tanah Genting Yang Menghubungkan Amerika Utara Selatan?", "id": "Tanah Genting Panama." },
  { "en": "Siapakah Yang Menemukan Proses Pasteurisasi?", "id": "Louis Pasteur." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Selangka Manusia?", "id": "Klavikula." },
  { "en": "Apa Nama Ibukota Kerajaan Inka Yang Terkenal?", "id": "Cusco." },
  { "en": "Apa Arti Istilah Musik 'Staccato'?", "id": "Terputus-Putus Atau Pendek." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep 'Allegory of the Cave'?", "id": "Plato." },
  { "en": "Apa Nama Ibukota Negara Uzbekistan?", "id": "Tashkent." },
  { "en": "Apa Nama Lagu Daerah Terkenal Dari Maluku?", "id": "Rasa Sayange." },
  { "en": "Siapa Penemu Hukum Gas Tentang Volume Dan Jumlah Mol?", "id": "Amedeo Avogadro." },
  { "en": "Apa Nama Teleskop Luar Angkasa Yang Menggantikan Hubble?", "id": "James Webb Space Telescope." },
  { "en": "Semenanjung Apa Yang Terletak Di Tenggara Eropa?", "id": "Semenanjung Balkan." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Mesin Cetak?", "id": "Johannes Gutenberg." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Lengan Atas?", "id": "Humerus." },
  { "en": "Apa Nama Dewa Utama Dalam Mitologi Suku Aztec?", "id": "Huitzilopochtli." },
  { "en": "Apa Arti Istilah Musik 'Fortissimo'?", "id": "Sangat Keras." },
  { "en": "Siapa Filsuf Yunani Yang Merupakan Guru Plato?", "id": "Socrates." },
  { "en": "Apa Nama Ibukota Negara Tajikistan?", "id": "Dushanbe." },
  { "en": "Apa Nama Alat Musik Gesek Dari Sulawesi Tengah?", "id": "Geso-Geso." },
  { "en": "Siapa Penemu Hukum Gas Gabungan Yang Ideal?", "id": "Benoît Paul Émile Clapeyron." },
  { "en": "Apa Nama Wahana Antariksa Penjelajah Planet Mars?", "id": "Perseverance Dan Curiosity." },
  { "en": "Apa Nama Semenanjung Tempat Negara Italia Berada?", "id": "Semenanjung Italia." },
  { "en": "Siapakah Yang Menemukan Pendingin Udara (AC) Modern?", "id": "Willis Carrier." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Paha Manusia?", "id": "Femur." },
  { "en": "Siapa Dewa Matahari Dalam Mitologi Suku Inka?", "id": "Inti." },
  { "en": "Apa Arti Istilah Musik 'Decrescendo'?", "id": "Semakin Lembut." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep 'Übermensch'?", "id": "Friedrich Nietzsche." },
  { "en": "Apa Nama Ibukota Negara Georgia?", "id": "Tbilisi." },
  { "en": "Apa Nama Seni Bela Diri Khas Indonesia?", "id": "Pencak Silat." },
  { "en": "Siapa Penemu Hukum Gas Tentang Tekanan Parsial?", "id": "John Dalton." },
  { "en": "Apa Nama Misi Yang Pertama Kali Mengorbit Mars?", "id": "Mariner 9." },
  { "en": "Semenanjung Apa Yang Terletak Di Asia Barat Daya?", "id": "Semenanjung Arab." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Bola Lampu Praktis?", "id": "Thomas Edison." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Kering Manusia?", "id": "Tibia." },
  { "en": "Siapa Nama Kaisar Terkenal Dari Kekaisaran Mali?", "id": "Mansa Musa." },
  { "en": "Apa Arti Istilah Musik 'Accelerando'?", "id": "Semakin Cepat." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Liberalisme?", "id": "John Locke." },
  { "en": "Apa Nama Ibukota Negara Azerbaijan?", "id": "Baku." },
  { "en": "Apa Nama Upacara Adat Kematian Suku Toraja?", "id": "Rambu Solo'." },
  { "en": "Siapa Penemu Hukum Difusi Gas Yang Terkenal?", "id": "Thomas Graham." },
  { "en": "Apa Nama Wahana Yang Mengunjungi Saturnus?", "id": "Cassini-Huygens." },
  { "en": "Apa Nama Tanah Genting Yang Memisahkan Teluk Siam?", "id": "Tanah Genting Kra." },
  { "en": "Siapakah Yang Menemukan Proses Vulkanisasi Karet?", "id": "Charles Goodyear." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Betis Manusia?", "id": "Fibula." },
  { "en": "Siapa Dewa Pencipta Dalam Mitologi Suku Maya?", "id": "Itzamna." },
  { "en": "Apa Arti Istilah Musik 'Pianissimo'?", "id": "Sangat Lembut." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep Etika Deontologis?", "id": "Immanuel Kant." },
  { "en": "Apa Nama Ibukota Negara Kazakhstan?", "id": "Astana." },
  { "en": "Apa Nama Rumah Adat Khas Suku Minangkabau?", "id": "Rumah Gadang." },
  { "en": "Hukum Gas Apa Yang Menjelaskan Hubungan Tekanan Suhu?", "id": "Hukum Gay-Lussac." },
  { "en": "Apa Nama Wahana Antariksa Yang Keluar Tata Surya?", "id": "Voyager 1 Dan Voyager 2." },
  { "en": "Semenanjung Apa Yang Membentuk Bagian Selatan India?", "id": "Semenanjung Deccan." },
  { "en": "Siapakah Yang Menemukan Baterai Listrik Pertama?", "id": "Alessandro Volta." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Pergelangan Tangan?", "id": "Karpal." },
  { "en": "Apa Nama Ibukota Kerajaan Ghana Kuno?", "id": "Koumbi Saleh." },
  { "en": "Apa Arti Istilah Musik 'Legato'?", "id": "Menyambung Tanpa Jeda." },
  { "en": "Siapa Filsuf Yang Merupakan Murid Aristoteles?", "id": "Alexander Agung." },
  { "en": "Apa Nama Ibukota Negara Armenia?", "id": "Yerevan." },
  { "en": "Apa Nama Tarian Selamat Datang Dari Papua?", "id": "Tari Yospan." },
  { "en": "Siapa Penemu Hukum Gas Hubungan Suhu Tekanan?", "id": "Guillaume Amontons." },
  { "en": "Apa Nama Misi Ke Pluto Pada Tahun 2015?", "id": "New Horizons." },
  { "en": "Semenanjung Apa Yang Menjadi Bagian Dari Turki?", "id": "Semenanjung Anatolia." },
  { "en": "Siapakah Yang Menemukan Sabuk Pengaman Tiga Titik?", "id": "Nils Bohlin." },
  { "en": "Apa Nama Ilmiah Untuk Tulang-Tulang Jari?", "id": "Falang." },
  { "en": "Siapa Nama Kaisar Terkenal Dari Kekaisaran Aksum?", "id": "Raja Ezana." },
  { "en": "Apa Arti Istilah Musik 'Vivace'?", "id": "Cepat Dan Bersemangat." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Paradoksnya?", "id": "Zeno dari Elea." },
  { "en": "Apa Nama Ibukota Negara Mongolia?", "id": "Ulaanbaatar." },
  { "en": "Apa Nama Keris Terkenal Milik Ken Arok?", "id": "Keris Mpu Gandring." },
  { "en": "Hukum Gas Siapakah Yang Menjadi Dasar Teori Kinetik?", "id": "Rudolf Clausius." },
  { "en": "Apa Nama Wahana Antariksa Pertama Yang Mengorbit Bumi?", "id": "Sputnik 1." },
  { "en": "Apa Nama Selat Yang Memisahkan Semenanjung Malaya Dan Sumatra?", "id": "Selat Malaka." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1970?", "id": "Aleksandr Solzhenitsyn." },
  { "en": "Kelenjar Apa Yang Dikenal Sebagai 'Master Gland'?", "id": "Kelenjar Pituitari." },
  { "en": "Siapa Raja Inggris Yang Digulingkan Dalam 'Glorious Revolution'?", "id": "Raja James II." },
  { "en": "Apa Istilah Seni Lukis Teknik Kontras Cahaya Gelap?", "id": "Chiaroscuro." },
  { "en": "Hukum Siapakah Yang Menjelaskan Hubungan Arus Dan Tegangan?", "id": "Hukum Ohm." },
  { "en": "Apa Nama Ibukota Negara Pantai Gading (Côte d'Ivoire)?", "id": "Yamoussoukro." },
  { "en": "Apa Nama Senjata Tradisional Berbentuk Cakar Dari Minangkabau?", "id": "Kerambit." },
  { "en": "Ikatan Kimia Apa Yang Terjadi Akibat Tarik-Menarik Ion?", "id": "Ikatan Ionik." },
  { "en": "Apa Nama Teleskop Radio Terbesar Di Dunia?", "id": "FAST (Five-hundred-meter Aperture)." },
  { "en": "Selat Apa Yang Menghubungkan Samudra Pasifik Dan Atlantik?", "id": "Selat Magellan." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1949?", "id": "William Faulkner." },
  { "en": "Kelenjar Apa Yang Menghasilkan Hormon Tiroksin?", "id": "Kelenjar Tiroid." },
  { "en": "Siapa Pemimpin Revolusi Iran Pada Tahun 1979?", "id": "Ayatollah Khomeini." },
  { "en": "Apa Istilah Untuk Teknik Cat Minyak Yang Tebal?", "id": "Impasto." },
  { "en": "Hukum Siapakah Yang Menjelaskan Perluasan Alam Semesta?", "id": "Hukum Hubble." },
  { "en": "Apa Nama Ibukota Negara Kamerun?", "id": "Yaoundé." },
  { "en": "Apa Nama Senjata Tradisional Berbentuk Perisai Dari Maluku?", "id": "Parang Salawaku." },
  { "en": "Ikatan Kimia Apa Yang Melibatkan Pemakaian Bersama Elektron?", "id": "Ikatan Kovalen." },
  { "en": "Apa Nama Observatorium Di Puncak Mauna Kea, Hawaii?", "id": "Observatorium Mauna Kea." },
  { "en": "Apa Nama Selat Yang Memisahkan India Dan Sri Lanka?", "id": "Selat Palk." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1964?", "id": "Jean-Paul Sartre." },
  { "en": "Kelenjar Apa Yang Menghasilkan Hormon Adrenalin?", "id": "Kelenjar Adrenal." },
  { "en": "Siapa Pemimpin Revolusi Bolshevik Di Rusia?", "id": "Vladimir Lenin." },
  { "en": "Apa Istilah Untuk Teknik Lukisan Dinding Basah?", "id": "Fresco." },
  { "en": "Hukum Siapakah Yang Menjelaskan Prinsip Pengapungan?", "id": "Hukum Archimedes." },
  { "en": "Apa Nama Ibukota Negara Zimbabwe?", "id": "Harare." },
  { "en": "Apa Nama Senjata Tradisional Berbentuk Sumpit Dari Dayak?", "id": "Sipet." },
  { "en": "Ikatan Kimia Apa Yang Terdapat Dalam Logam?", "id": "Ikatan Logam." },
  { "en": "Apa Nama Teleskop Luar Angkasa Pencari Planet Ekstrasurya?", "id": "Teleskop Kepler." },
  { "en": "Selat Apa Yang Menghubungkan Laut Hitam Ke Laut Marmara?", "id": "Selat Bosporus." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1957?", "id": "Albert Camus." },
  { "en": "Kelenjar Apa Yang Menghasilkan Hormon Insulin?", "id": "Pankreas." },
  { "en": "Siapa Pemimpin Revolusi Tiongkok Pada Tahun 1911?", "id": "Sun Yat-sen." },
  { "en": "Apa Istilah Untuk Seni Grafis Cetak Saring?", "id": "Serigrafi." },
  { "en": "Hukum Siapakah Yang Menjelaskan Gerak Planet?", "id": "Hukum Kepler." },
  { "en": "Apa Nama Ibukota Negara Madagaskar?", "id": "Antananarivo." },
  { "en": "Apa Nama Senjata Tradisional Mirip Trisula Dari Madura?", "id": "Clurit." },
  { "en": "Ikatan Apa Yang Terbentuk Antara Molekul Air?", "id": "Ikatan Hidrogen." },
  { "en": "Apa Nama Teleskop Radio Besar Di Puerto Riko?", "id": "Observatorium Arecibo." },
  { "en": "Apa Nama Selat Yang Memisahkan Greenland Dan Kanada?", "id": "Selat Davis." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 2016?", "id": "Bob Dylan." },
  { "en": "Kelenjar Apa Yang Menghasilkan Hormon Melatonin?", "id": "Kelenjar Pineal." },
  { "en": "Siapa Pemimpin Revolusi Kemerdekaan Amerika Latin?", "id": "Simón Bolívar." },
  { "en": "Apa Istilah Untuk Lukisan Dengan Cat Air?", "id": "Aquarel." },
  { "en": "Hukum Siapakah Yang Menjelaskan Kekekalan Massa?", "id": "Hukum Lavoisier." },
  { "en": "Apa Nama Ibukota Negara Angola?", "id": "Luanda." },
  { "en": "Apa Nama Senjata Tradisional Khas Suku Sunda?", "id": "Kujang." },
  { "en": "Ikatan Apa Yang Didasari Gaya Tarik Antar Molekul?", "id": "Gaya Van der Waals." },
  { "en": "Apa Nama Teleskop Luar Angkasa Yang Mendahului Hubble?", "id": "Skylab." },
  { "en": "Selat Apa Yang Menghubungkan Teluk Persia Dan Teluk Oman?", "id": "Selat Hormuz." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1993?", "id": "Toni Morrison." },
  { "en": "Kelenjar Apa Yang Berfungsi Dalam Sistem Imun?", "id": "Kelenjar Timus." },
  { "en": "Siapa Pemimpin Revolusi Perancis Yang Terkenal?", "id": "Maximilien Robespierre." },
  { "en": "Apa Istilah Untuk Seni Melukis Potret Diri?", "id": "Swafoto (Self-Portrait)." },
  { "en": "Hukum Siapakah Yang Menjelaskan Tekanan Fluida?", "id": "Hukum Pascal." },
  { "en": "Apa Nama Ibukota Negara Mozambik?", "id": "Maputo." },
  { "en": "Apa Nama Senjata Tradisional Dari Papua?", "id": "Panah Dan Busur." },
  { "en": "Apa Jenis Ikatan Kimia Terkuat Secara Umum?", "id": "Ikatan Kovalen." },
  { "en": "Apa Nama Observatorium Tertua Di Indonesia?", "id": "Observatorium Bosscha." },
  { "en": "Apa Nama Selat Yang Memisahkan Pulau-Pulau Jepang?", "id": "Selat Tsugaru." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1986?", "id": "Wole Soyinka." },
  { "en": "Kelenjar Apa Yang Menghasilkan Hormon Pertumbuhan?", "id": "Kelenjar Pituitari." },
  { "en": "Siapa Pemimpin Gerakan Kemerdekaan India?", "id": "Mahatma Gandhi." },
  { "en": "Apa Istilah Untuk Seni Menggambar Objek Mati?", "id": "Still Life." },
  { "en": "Hukum Siapakah Yang Menjelaskan Sifat Elastisitas?", "id": "Hukum Hooke." },
  { "en": "Apa Nama Ibukota Negara Ethiopia?", "id": "Addis Ababa." },
  { "en": "Apa Nama Senjata Tradisional Dari Sulawesi Selatan?", "id": "Badik." },
  { "en": "Apa Jenis Ikatan Kimia Terlemah Secara Umum?", "id": "Gaya London." },
  { "en": "Apa Nama Teleskop Yang Pertama Kali Melihat Cincin Saturnus?", "id": "Teleskop Galileo." },
  { "en": "Selat Apa Yang Menghubungkan Laut Merah Dan Laut Tengah?", "id": "Terusan Suez." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1925?", "id": "George Bernard Shaw." },
  { "en": "Kelenjar Apa Yang Mengontrol Metabolisme Kalsium?", "id": "Kelenjar Paratiroid." },
  { "en": "Siapa Pemimpin Revolusi Industri Di Inggris?", "id": "Tidak Ada Pemimpin Tunggal." },
  { "en": "Apa Istilah Untuk Seni Melukis Pemandangan Alam?", "id": "Lukisan Pemandangan." },
  { "en": "Hukum Siapakah Yang Menjelaskan Spektrum Benda Hitam?", "id": "Hukum Planck." },
  { "en": "Apa Nama Ibukota Negara Botswana?", "id": "Gaborone." },
  { "en": "Apa Nama Senjata Tradisional Dari Riau?", "id": "Pedang Jenawi." },
  { "en": "Apa Ikatan Yang Menyatukan Atom Dalam Molekul Gula?", "id": "Ikatan Glikosidik." },
  { "en": "Apa Nama Teleskop Luar Angkasa Pengamat Sinar-X?", "id": "Chandra X-ray Observatory." },
  { "en": "Apa Nama Selat Yang Memisahkan Britania Raya Dan Irlandia?", "id": "Selat Utara." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 2007?", "id": "Doris Lessing." },
  { "en": "Kelenjar Endokrin Apa Yang Terletak Di Otak?", "id": "Pineal Dan Pituitari." },
  { "en": "Siapa Pemimpin Revolusi Haiti Yang Terkenal?", "id": "Toussaint Louverture." },
  { "en": "Apa Istilah Untuk Seni Cetak Dari Blok Kayu?", "id": "Cukil Kayu (Woodcut)." },
  { "en": "Hukum Siapakah Yang Menjelaskan Efek Fotolistrik?", "id": "Albert Einstein." },
  { "en": "Apa Nama Ibukota Negara Namibia?", "id": "Windhoek." },
  { "en": "Apa Nama Senjata Tradisional Dari Lampung?", "id": "Terapang." },
  { "en": "Apa Ikatan Yang Menghubungkan Asam Amino?", "id": "Ikatan Peptida." },
  { "en": "Apa Nama Teleskop Luar Angkasa Pengamat Sinar Gamma?", "id": "Compton Gamma Ray Observatory." },
  { "en": "Di Benua Manakah Letak Gurun Atacama?", "id": "Amerika Selatan." },
  { "en": "Siapa Komposer Yang Menciptakan Opera 'The Magic Flute'?", "id": "Wolfgang Amadeus Mozart." },
  { "en": "Bagian Otak Mana Yang Mengontrol Keseimbangan Dan Koordinasi?", "id": "Otak Kecil (Serebelum)." },
  { "en": "Peradaban Kuno Mana Yang Dikenal Sebagai Pelaut Ulung?", "id": "Bangsa Fenisia." },
  { "en": "Apa Majas Yang Menggunakan Kata 'Seperti' Atau 'Bagaikan'?", "id": "Simile (Perumpamaan)." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Golongan Darah Manusia?", "id": "Karl Landsteiner." },
  { "en": "Apa Nama Ibukota Negara Guyana?", "id": "Georgetown." },
  { "en": "Dari Provinsi Manakah Asal Tari Merak?", "id": "Jawa Barat." },
  { "en": "Apa Gaya Fundamental Terkuat Di Alam Semesta?", "id": "Gaya Nuklir Kuat." },
  { "en": "Siapakah Wanita Pertama Yang Pergi Ke Luar Angkasa?", "id": "Valentina Tereshkova." },
  { "en": "Di Benua Manakah Letak Gurun Namib?", "id": "Afrika." },
  { "en": "Siapa Komposer Yang Menciptakan Simfoni No. 5?", "id": "Ludwig van Beethoven." },
  { "en": "Bagian Otak Mana Yang Mengatur Fungsi Vital Tubuh?", "id": "Batang Otak." },
  { "en": "Peradaban Kuno Mana Yang Terkenal Dengan Tulisan Paku?", "id": "Bangsa Sumeria." },
  { "en": "Apa Majas Yang Memberi Sifat Manusia Pada Benda Mati?", "id": "Personifikasi." },
  { "en": "Siapakah Yang Pertama Kali Mengamati Sel Hidup?", "id": "Antonie van Leeuwenhoek." },
  { "en": "Apa Nama Ibukota Negara Suriname?", "id": "Paramaribo." },
  { "en": "Dari Provinsi Manakah Asal Tari Piring?", "id": "Sumatera Barat." },
  { "en": "Apa Gaya Fundamental Yang Bertanggung Jawab Atas Peluruhan Radioaktif?", "id": "Gaya Nuklir Lemah." },
  { "en": "Siapakah Manusia Pertama Yang Melakukan Spacewalk?", "id": "Alexei Leonov." },
  { "en": "Di Asia Tengah, Di Manakah Letak Gurun Karakum?", "id": "Turkmenistan." },
  { "en": "Siapa Komposer Yang Menciptakan 'The Blue Danube Waltz'?", "id": "Johann Strauss II." },
  { "en": "Apa Nama Sel Saraf Dasar Dalam Sistem Saraf?", "id": "Neuron." },
  { "en": "Peradaban Kuno Mana Yang Berkembang Di Pulau Kreta?", "id": "Peradaban Minoa." },
  { "en": "Apa Majas Yang Melebih-Lebihkan Sesuatu Secara Ekstrem?", "id": "Hiperbola." },
  { "en": "Siapakah Yang Merumuskan Hukum Termodinamika?", "id": "Sadi Carnot." },
  { "en": "Apa Nama Ibukota Negara Belize?", "id": "Belmopan." },
  { "en": "Dari Provinsi Manakah Asal Tari Jaipong?", "id": "Jawa Barat." },
  { "en": "Apa Gaya Fundamental Yang Bekerja Antar Muatan Listrik?", "id": "Gaya Elektromagnetik." },
  { "en": "Siapakah Wanita Amerika Pertama Di Luar Angkasa?", "id": "Sally Ride." },
  { "en": "Gurun Berbatu Terbesar Di Australia Adalah?", "id": "Gurun Gibson." },
  { "en": "Siapa Komposer Rusia Yang Menciptakan Balet 'The Nutcracker'?", "id": "Pyotr Ilyich Tchaikovsky." },
  { "en": "Bagian Otak Mana Yang Berperan Dalam Memori Dan Emosi?", "id": "Sistem Limbik." },
  { "en": "Peradaban Kuno Mana Yang Terkenal Dengan Patung Moai?", "id": "Peradaban Rapa Nui." },
  { "en": "Apa Majas Yang Menggunakan Kata-Kata Berlawanan Arti?", "id": "Oksimoron." },
  { "en": "Siapakah Yang Menemukan Struktur Heliks Ganda DNA?", "id": "Watson Dan Crick." },
  { "en": "Apa Nama Ibukota Negara Nikaragua?", "id": "Managua." },
  { "en": "Dari Provinsi Manakah Asal Tari Saman?", "id": "Aceh." },
  { "en": "Apa Gaya Fundamental Terlemah Di Alam Semesta?", "id": "Gaya Gravitasi." },
  { "en": "Siapakah Orang Pertama Yang Mengorbit Bumi?", "id": "Yuri Gagarin." },
  { "en": "Di Amerika Utara, Di Manakah Letak Gurun Mojave?", "id": "Amerika Serikat." },
  { "en": "Siapa Komposer Italia Pencipta Opera 'Aida'?", "id": "Giuseppe Verdi." },
  { "en": "Apa Bagian Otak Terbesar Yang Berfungsi Untuk Berpikir?", "id": "Otak Besar (Serebrum)." },
  { "en": "Peradaban Kuno Mana Yang Mendiami Lembah Sungai Indus?", "id": "Peradaban Lembah Indus." },
  { "en": "Apa Majas Yang Menggantikan Nama Benda Dengan Ciri Khasnya?", "id": "Metonimia." },
  { "en": "Siapakah Yang Menemukan Vaksin Polio Oral?", "id": "Albert Sabin." },
  { "en": "Apa Nama Ibukota Negara Guatemala?", "id": "Kota Guatemala." },
  { "en": "Dari Provinsi Manakah Asal Tari Kecak?", "id": "Bali." },
  { "en": "Apa Nama Partikel Pembawa Gaya Elektromagnetik?", "id": "Foton." },
  { "en": "Siapakah Manusia Pertama Yang Menginjakkan Kaki Di Bulan?", "id": "Neil Armstrong." },
  { "en": "Di Manakah Letak Gurun Thar Yang Luas?", "id": "India Dan Pakistan." },
  { "en": "Siapa Komposer Yang Menciptakan Opera 'Carmen'?", "id": "Georges Bizet." },
  { "en": "Apa Nama Jarak Antar Dua Sel Saraf?", "id": "Sinapsis." },
  { "en": "Peradaban Kuno Mana Yang Terkenal Dengan Kepala Batu Raksasa?", "id": "Peradaban Olmek." },
  { "en": "Apa Majas Yang Menggambarkan Benda Seolah-Olah Hidup?", "id": "Animisme." },
  { "en": "Siapakah Yang Menemukan Elektron?", "id": "J.J. Thomson." },
  { "en": "Apa Nama Ibukota Negara Honduras?", "id": "Tegucigalpa." },
  { "en": "Dari Daerah Manakah Asal Tari Yapong?", "id": "DKI Jakarta." },
  { "en": "Apa Nama Partikel Pembawa Gaya Gravitasi (Teoretis)?", "id": "Graviton." },
  { "en": "Siapakah Orang Kedua Yang Berjalan Di Bulan?", "id": "Buzz Aldrin." },
  { "en": "Di Manakah Letak Gurun Sonoran Berada?", "id": "Amerika Utara." },
  { "en": "Siapa Komposer Yang Menciptakan 'Ride of the Valkyries'?", "id": "Richard Wagner." },
  { "en": "Apa Bagian Otak Yang Mengontrol Denyut Jantung?", "id": "Medula Oblongata." },
  { "en": "Peradaban Kuno Mana Yang Terkenal Dengan Kode Hukumnya?", "id": "Babilonia." },
  { "en": "Apa Majas Yang Menggunakan Perbandingan Langsung Tanpa Kata Sambung?", "id": "Metafora." },
  { "en": "Siapakah Yang Menemukan Inti Atom (Nukleus)?", "id": "Ernest Rutherford." },
  { "en": "Apa Nama Ibukota Negara El Salvador?", "id": "San Salvador." },
  { "en": "Dari Provinsi Manakah Asal Tari Gong?", "id": "Kalimantan Timur." },
  { "en": "Apa Nama Partikel Pembawa Gaya Nuklir Kuat?", "id": "Gluon." },
  { "en": "Siapakah Komandan Misi Apollo 11 Ke Bulan?", "id": "Neil Armstrong." },
  { "en": "Di Manakah Letak Gurun Pasir Putih (White Sands)?", "id": "New Mexico, AS." },
  { "en": "Siapa Komposer Yang Menciptakan 'Boléro'?", "id": "Maurice Ravel." },
  { "en": "Apa Lobus Otak Yang Berfungsi Untuk Penglihatan?", "id": "Lobus Oksipital." },
  { "en": "Peradaban Kuno Mana Yang Membangun Ziggurat?", "id": "Sumeria, Babilonia, Asiria." },
  { "en": "Apa Majas Yang Menyatakan Sesuatu Dengan Kata Kebalikannya?", "id": "Litotes." },
  { "en": "Siapakah Yang Mengusulkan Model Atom Planet?", "id": "Niels Bohr." },
  { "en": "Apa Nama Ibukota Negara Kosta Rika?", "id": "San José." },
  { "en": "Dari Provinsi Manakah Asal Tari Tor-Tor?", "id": "Sumatera Utara." },
  { "en": "Apa Nama Partikel Pembawa Gaya Nuklir Lemah?", "id": "Boson W Dan Z." },
  { "en": "Siapakah Orang Inggris Pertama Di Luar Angkasa?", "id": "Helen Sharman." },
  { "en": "Di Tiongkok, Di Manakah Letak Gurun Taklamakan?", "id": "Xinjiang." },
  { "en": "Siapa Komposer Yang Menciptakan 'Also sprach Zarathustra'?", "id": "Richard Strauss." },
  { "en": "Apa Lobus Otak Yang Berfungsi Untuk Pendengaran?", "id": "Lobus Temporal." },
  { "en": "Peradaban Kuno Mana Yang Terkenal Dengan Sistem Tulisannya?", "id": "Mesir Kuno (Hieroglif)." },
  { "en": "Apa Majas Yang Mengulang Kata Di Awal Kalimat?", "id": "Anafora." },
  { "en": "Siapakah Yang Menemukan Neutron?", "id": "James Chadwick." },
  { "en": "Apa Nama Ibukota Negara Panama?", "id": "Kota Panama." },
  { "en": "Dari Provinsi Manakah Asal Tari Serimpi?", "id": "Jawa Tengah." },
  { "en": "Apa Nama Gaya Yang Menarik Benda Ke Pusat Bumi?", "id": "Gaya Gravitasi." },
  { "en": "Siapakah Orang Kanada Pertama Di Luar Angkasa?", "id": "Marc Garneau." },
  { "en": "Di Negara Manakah Letak Pulau Baffin?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1964?", "id": "Martin Luther King Jr." },
  { "en": "Pembuluh Darah Apa Yang Membawa Darah Kembali Ke Jantung?", "id": "Vena." },
  { "en": "Apa Nama Periode Pencerahan Di Eropa Abad Ke-18?", "id": "Abad Pencerahan (Enlightenment)." },
  { "en": "Apa Nama Penopang Eksternal Khas Arsitektur Gotik?", "id": "Penopang Layang (Flying Buttress)." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Tekanan Pascal?", "id": "Blaise Pascal." },
  { "en": "Apa Nama Ibukota Negara Nepal?", "id": "Kathmandu." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Papeda?", "id": "Papua Dan Maluku." },
  { "en": "Apa Istilah Perubahan Zat Padat Langsung Menjadi Gas?", "id": "Sublimasi." },
  { "en": "Apa Nama Wahana Berawak Pertama Uni Soviet?", "id": "Vostok 1." },
  { "en": "Di Negara Manakah Letak Pulau Madagaskar?", "id": "Madagaskar." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1983?", "id": "Lech Wałęsa." },
  { "en": "Apa Nama Pembuluh Darah Terbesar Di Tubuh Manusia?", "id": "Aorta." },
  { "en": "Apa Nama Gerakan Reformasi Agama Di Eropa?", "id": "Reformasi Protestan." },
  { "en": "Apa Nama Pilar Gaya Arsitektur Yunani Paling Sederhana?", "id": "Orde Dorik." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Daya Watt?", "id": "James Watt." },
  { "en": "Apa Nama Ibukota Negara Sri Lanka?", "id": "Sri Jayawardenepura Kotte." },
  { "en": "Dari Kota Manakah Asal Makanan Khas Gudeg?", "id": "Yogyakarta." },
  { "en": "Apa Istilah Perubahan Zat Gas Langsung Menjadi Padat?", "id": "Deposisi." },
  { "en": "Apa Nama Wahana Bulan Berawak Pertama Amerika?", "id": "Apollo 11." },
  { "en": "Di Negara Manakah Letak Pulau Honshu?", "id": "Jepang." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1984?", "id": "Desmond Tutu." },
  { "en": "Apa Nama Pembuluh Darah Terkecil Di Tubuh Manusia?", "id": "Kapiler." },
  { "en": "Apa Nama Periode Kelahiran Kembali Seni Dan Sains?", "id": "Renaisans." },
  { "en": "Apa Nama Pilar Gaya Arsitektur Yunani Dengan Ulir?", "id": "Orde Ionik." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Frekuensi Hertz?", "id": "Heinrich Hertz." },
  { "en": "Apa Nama Ibukota Negara Bangladesh?", "id": "Dhaka." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Bika Ambon?", "id": "Medan." },
  { "en": "Apa Istilah Perubahan Zat Cair Menjadi Gas?", "id": "Penguapan (Evaporasi)." },
  { "en": "Apa Nama Robot Penjelajah Bulan Uni Soviet?", "id": "Lunokhod." },
  { "en": "Di Negara Manakah Letak Pulau Sumatra?", "id": "Indonesia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1989?", "id": "Dalai Lama Ke-14." },
  { "en": "Apa Komponen Darah Yang Berperan Dalam Pembekuan?", "id": "Trombosit." },
  { "en": "Apa Nama Periode Sejarah Antara Abad Kuno Dan Modern?", "id": "Abad Pertengahan." },
  { "en": "Apa Nama Pilar Gaya Arsitektur Yunani Paling Rumit?", "id": "Orde Korintus." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Gaya Newton?", "id": "Isaac Newton." },
  { "en": "Apa Nama Ibukota Negara Pakistan?", "id": "Islamabad." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Pempek?", "id": "Palembang." },
  { "en": "Apa Istilah Perubahan Zat Gas Menjadi Cair?", "id": "Kondensasi." },
  { "en": "Apa Nama Misi Pendaratan Di Bulan Tanpa Awak Pertama?", "id": "Luna 9 (Soviet)." },
  { "en": "Di Samudra Manakah Letak Pulau Paskah?", "id": "Samudra Pasifik." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1991?", "id": "Aung San Suu Kyi." },
  { "en": "Apa Nama Cairan Kuning Pucat Dalam Darah?", "id": "Plasma Darah." },
  { "en": "Apa Nama Zaman Es Besar Terakhir Di Bumi?", "id": "Pleistosen." },
  { "en": "Apa Nama Atap Berbentuk Setengah Bola?", "id": "Kubah (Dome)." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Energi Joule?", "id": "James Prescott Joule." },
  { "en": "Apa Nama Ibukota Negara Kamboja?", "id": "Phnom Penh." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Kerak Telor?", "id": "Jakarta (Betawi)." },
  { "en": "Apa Istilah Perubahan Zat Cair Menjadi Padat?", "id": "Pembekuan." },
  { "en": "Apa Nama Kendaraan Yang Digunakan Di Bulan?", "id": "Lunar Roving Vehicle (LRV)." },
  { "en": "Di Negara Manakah Letak Pulau Kalimantan?", "id": "Indonesia, Malaysia, Brunei." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1994?", "id": "Arafat, Peres, Rabin." },
  { "en": "Apa Pembuluh Arteri Terbesar Yang Keluar Dari Jantung?", "id": "Aorta." },
  { "en": "Apa Nama Periode Perang Dingin Antara AS Dan Soviet?", "id": "Perang Dingin." },
  { "en": "Apa Nama Hiasan Horizontal Di Atas Kolom Bangunan?", "id": "Frieze." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Tegangan Listrik Volt?", "id": "Alessandro Volta." },
  { "en": "Apa Nama Ibukota Negara Myanmar?", "id": "Naypyidaw." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Ayam Betutu?", "id": "Bali." },
  { "en": "Apa Istilah Perubahan Zat Gas Menjadi Plasma?", "id": "Ionisasi." },
  { "en": "Apa Nama Program Antariksa Berawak Pertama Amerika?", "id": "Program Merkuri." },
  { "en": "Di Negara Manakah Letak Pulau Greenland?", "id": "Denmark." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 2002?", "id": "Jimmy Carter." },
  { "en": "Apa Nama Katup Antara Serambi Kiri Dan Bilik Kiri?", "id": "Katup Mitral." },
  { "en": "Apa Nama Zaman Penemuan Kembali Budaya Yunani Romawi?", "id": "Renaisans." },
  { "en": "Apa Nama Jendela Melingkar Besar Di Gereja Gotik?", "id": "Jendela Mawar (Rose Window)." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Hambatan Listrik Ohm?", "id": "Georg Ohm." },
  { "en": "Apa Nama Ibukota Negara Laos?", "id": "Vientiane." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Soto Banjar?", "id": "Kalimantan Selatan." },
  { "en": "Apa Istilah Perubahan Zat Plasma Menjadi Gas?", "id": "Rekombinasi." },
  { "en": "Apa Nama Misi Yang Membawa Manusia Mengorbit Bulan?", "id": "Apollo 8." },
  { "en": "Di Samudra Manakah Letak Kepulauan Galapagos?", "id": "Samudra Pasifik." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 2009?", "id": "Barack Obama." },
  { "en": "Apa Sirkulasi Darah Yang Menuju Paru-Paru?", "id": "Sirkulasi Pulmonal." },
  { "en": "Apa Nama Era Kekuasaan Kaisar Di Jepang?", "id": "Zaman Kofun Hingga Modern." },
  { "en": "Apa Nama Puncak Segitiga Di Atas Pintu Masuk?", "id": "Pedimen." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Suhu Kelvin?", "id": "Lord Kelvin." },
  { "en": "Apa Nama Ibukota Negara Brunei Darussalam?", "id": "Bandar Seri Begawan." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Coto Makassar?", "id": "Sulawesi Selatan." },
  { "en": "Apa Istilah Perubahan Zat Padat Menjadi Cair?", "id": "Mencair (Fusion)." },
  { "en": "Apa Nama Program Penerus Proyek Apollo?", "id": "Program Pesawat Ulang Alik." },
  { "en": "Di Negara Manakah Letak Pulau Britania Raya?", "id": "Inggris Raya." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 2014?", "id": "Malala Yousafzai, Kailash Satyarthi." },
  { "en": "Apa Sirkulasi Darah Yang Menuju Seluruh Tubuh?", "id": "Sirkulasi Sistemik." },
  { "en": "Apa Nama Periode Sejarah Kekaisaran Romawi Timur?", "id": "Kekaisaran Bizantium." },
  { "en": "Apa Nama Teras Berkolom Di Depan Bangunan?", "id": "Portico." },
  { "en": "Siapa Ilmuwan Di Balik Satuan Arus Listrik Ampere?", "id": "André-Marie Ampère." },
  { "en": "Apa Nama Ibukota Negara Timor Leste?", "id": "Dili." },
  { "en": "Dari Daerah Manakah Asal Makanan Khas Arsik?", "id": "Sumatera Utara." },
  { "en": "Apa Nama Titik Suhu Dimana Zat Mulai Mendidih?", "id": "Titik Didih." },
  { "en": "Apa Nama Program Antariksa Untuk Mendarat Di Mars?", "id": "Program Artemis." },
  { "en": "Sungai Nil Bermuara Di Laut Mana?", "id": "Laut Mediterania." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Pendiri Stoikisme?", "id": "Zeno dari Citium." },
  { "en": "Organ Apa Yang Menghubungkan Kerongkongan Dengan Lambung?", "id": "Esofagus." },
  { "en": "Apa Nama Dinasti Yang Memerintah Inggris Pada Abad Ke-16?", "id": "Dinasti Tudor." },
  { "en": "Keluarga Alat Musik Apa Termasuk Biola Dan Cello?", "id": "Alat Musik Gesek." },
  { "en": "Siapa Ilmuwan Wanita Yang Meneliti Radioaktivitas?", "id": "Marie Curie." },
  { "en": "Apa Nama Ibukota Negara Oman?", "id": "Muskat." },
  { "en": "Apa Nama Pakaian Adat Tradisional Suku Bugis?", "id": "Baju Bodo." },
  { "en": "Apa Nama Mesin Sederhana Berupa Batang Dan Penumpu?", "id": "Tuas (Lever)." },
  { "en": "Apa Nama Nebula Terkenal Di Rasi Bintang Orion?", "id": "Nebula Orion." },
  { "en": "Sungai Amazon Bermuara Di Samudra Mana?", "id": "Samudra Atlantik." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Pendiri Epikureanisme?", "id": "Epikuros." },
  { "en": "Di Organ Mana Sebagian Besar Nutrisi Diserap?", "id": "Usus Halus." },
  { "en": "Apa Nama Dinasti Terakhir Yang Memerintah Tiongkok?", "id": "Dinasti Qing." },
  { "en": "Keluarga Alat Musik Apa Termasuk Flut Dan Klarinet?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Siapa Ilmuwan Yang Mengembangkan Sistem Klasifikasi Biologis?", "id": "Carolus Linnaeus." },
  { "en": "Apa Nama Ibukota Negara Yordania?", "id": "Amman." },
  { "en": "Apa Nama Penutup Kepala Pria Khas Jawa?", "id": "Blangkon." },
  { "en": "Apa Nama Mesin Sederhana Yang Menggunakan Tali Dan Roda?", "id": "Katrol (Pulley)." },
  { "en": "Apa Nama Nebula Yang Dikenal Sebagai 'Pilar Penciptaan'?", "id": "Nebula Elang." },
  { "en": "Sungai Mississippi Bermuara Di Teluk Mana?", "id": "Teluk Meksiko." },
  { "en": "Aliran Filsafat Apa Yang Diasosiasikan Dengan Diogenes?", "id": "Sinisme." },
  { "en": "Organ Apa Yang Berfungsi Menyimpan Empedu?", "id": "Kantung Empedu." },
  { "en": "Apa Nama Dinasti Yang Memerintah Rusia Selama 300 Tahun?", "id": "Dinasti Romanov." },
  { "en": "Keluarga Alat Musik Apa Termasuk Trompet Dan Trombon?", "id": "Alat Musik Tiup Logam." },
  { "en": "Siapa Ilmuwan Yang Merumuskan Teori Relativitas?", "id": "Albert Einstein." },
  { "en": "Apa Nama Ibukota Negara Yaman?", "id": "Sana'a." },
  { "en": "Apa Nama Kain Batik Dengan Pewarnaan Alami Dari Jawa?", "id": "Batik Sogan." },
  { "en": "Apa Nama Mesin Sederhana Yang Memiliki Poros Dan Roda?", "id": "Roda Dan Poros." },
  { "en": "Apa Nama Galaksi Spiral Berpalang Tempat Kita Tinggal?", "id": "Galaksi Bima Sakti." },
  { "en": "Sungai Yangtze Bermuara Di Laut Mana?", "id": "Laut Tiongkok Timur." },
  { "en": "Aliran Filsafat Apa Yang Berfokus Pada Kebebasan Individu?", "id": "Eksistensialisme." },
  { "en": "Apa Nama Bagian Pertama Dari Usus Halus?", "id": "Duodenum (Usus 12 Jari)." },
  { "en": "Apa Nama Dinasti Yang Membangun Tembok Besar Tiongkok?", "id": "Dinasti Ming (sebagian besar)." },
  { "en": "Keluarga Alat Musik Apa Termasuk Drum Dan Simbal?", "id": "Alat Musik Perkusi." },
  { "en": "Siapa Ilmuwan Yang Menemukan Hukum Gravitasi?", "id": "Isaac Newton." },
  { "en": "Apa Nama Ibukota Negara Bahrain?", "id": "Manama." },
  { "en": "Apa Nama Pakaian Adat Resmi Pria Indonesia?", "id": "Batik." },
  { "en": "Apa Nama Mesin Sederhana Yang Berupa Permukaan Miring?", "id": "Bidang Miring." },
  { "en": "Apa Nama Gugus Bintang Terdekat Dari Tata Surya?", "id": "Alpha Centauri." },
  { "en": "Sungai Gangga Bermuara Di Teluk Mana?", "id": "Teluk Benggala." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Filsafat Barat?", "id": "Thales dari Miletus." },
  { "en": "Apa Nama Bagian Terakhir Dari Usus Besar?", "id": "Rektum." },
  { "en": "Apa Nama Dinasti Kekaisaran Pertama Di Jepang?", "id": "Dinasti Yamato." },
  { "en": "Gitar Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Petik." },
  { "en": "Siapa Ilmuwan Yang Mengemukakan Teori Evolusi?", "id": "Charles Darwin." },
  { "en": "Apa Nama Ibukota Negara Uni Emirat Arab?", "id": "Abu Dhabi." },
  { "en": "Apa Nama Selendang Kain Khas Palembang?", "id": "Songket." },
  { "en": "Apa Nama Mesin Sederhana Yang Membelah Benda?", "id": "Baji (Wedge)." },
  { "en": "Apa Nama Nebula Terdekat Dengan Planet Bumi?", "id": "Nebula Helix." },
  { "en": "Sungai Kongo Bermuara Di Samudra Mana?", "id": "Samudra Atlantik." },
  { "en": "Siapa Filsuf Yang Menulis 'The Republic'?", "id": "Plato." },
  { "en": "Organ Apa Yang Memproduksi Asam Lambung?", "id": "Lambung." },
  { "en": "Apa Nama Dinasti Feodal Terakhir Di Jepang?", "id": "Keshogunan Tokugawa." },
  { "en": "Piano Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Papan Tuts." },
  { "en": "Siapa Ilmuwan Yang Menemukan Penisilin?", "id": "Alexander Fleming." },
  { "en": "Apa Nama Ibukota Negara Kuwait?", "id": "Kota Kuwait." },
  { "en": "Apa Nama Pakaian Adat Pria Dari Betawi?", "id": "Baju Sadariah." },
  { "en": "Apa Nama Mesin Sederhana Yang Berbentuk Ulir?", "id": "Sekrup (Screw)." },
  { "en": "Apa Nama Galaksi Kerdil Yang Mengorbit Bima Sakti?", "id": "Awan Magellan." },
  { "en": "Sungai Mekong Bermuara Di Laut Mana?", "id": "Laut Tiongkok Selatan." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Metode Dialektika?", "id": "Socrates." },
  { "en": "Apa Fungsi Utama Dari Usus Besar?", "id": "Menyerap Air Dan Elektrolit." },
  { "en": "Apa Nama Dinasti Islam Pertama Di India?", "id": "Dinasti Mamluk (Delhi)." },
  { "en": "Harpa Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Petik." },
  { "en": "Siapa Ilmuwan Yang Menemukan Sinar-X?", "id": "Wilhelm Röntgen." },
  { "en": "Apa Nama Ibukota Negara Lebanon?", "id": "Beirut." },
  { "en": "Apa Nama Pakaian Adat Wanita Dari Jawa?", "id": "Kebaya." },
  { "en": "Apa Kombinasi Dari Dua Atau Lebih Mesin Sederhana?", "id": "Mesin Majemuk." },
  { "en": "Apa Nama Gugus Bintang Yang Dikenal Sebagai 'Seven Sisters'?", "id": "Pleiades." },
  { "en": "Sungai Danube Bermuara Di Laut Mana?", "id": "Laut Hitam." },
  { "en": "Aliran Filsafat Apa Yang Menekankan Pengalaman Indrawi?", "id": "Empirisme." },
  { "en": "Organ Apa Yang Menempel Pada Usus Besar?", "id": "Usus Buntu (Appendix)." },
  { "en": "Apa Nama Dinasti Yang Memerintah Kekaisaran Persia?", "id": "Dinasti Achaemenid." },
  { "en": "Saksofon Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Siapa Ilmuwan Yang Mengembangkan Teori Kuantum?", "id": "Max Planck." },
  { "en": "Apa Nama Ibukota Negara Irak?", "id": "Baghdad." },
  { "en": "Apa Nama Sarung Khas Dari Bugis?", "id": "Sarung Bugis." },
  { "en": "Tuas Jenis Pertama Memiliki Titik Tumpu Di Mana?", "id": "Antara Beban Dan Kuasa." },
  { "en": "Apa Nama Sisa-Sisa Ledakan Bintang Supernova?", "id": "Sisa Supernova." },
  { "en": "Sungai Rhein Bermuara Di Laut Mana?", "id": "Laut Utara." },
  { "en": "Aliran Filsafat Apa Yang Menekankan Logika Dan Akal?", "id": "Rasionalisme." },
  { "en": "Di Bagian Mana Pencernaan Karbohidrat Dimulai?", "id": "Mulut." },
  { "en": "Apa Nama Dinasti Yang Memerintah Kekaisaran Mongol?", "id": "Dinasti Yuan (Di Tiongkok)." },
  { "en": "Akordeon Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup." },
  { "en": "Siapa Ilmuwan Yang Terkenal Dengan Kucing Schrödinger?", "id": "Erwin Schrödinger." },
  { "en": "Apa Nama Ibukota Negara Suriah?", "id": "Damaskus." },
  { "en": "Apa Nama Kain Tenun Khas Dari Sumba?", "id": "Tenun Ikat Sumba." },
  { "en": "Contoh Tuas Jenis Kedua Adalah Apa?", "id": "Gerobak Dorong." },
  { "en": "Apa Nama Nebula Yang Menyerupai Kepala Kuda?", "id": "Nebula Kepala Kuda." },
  { "en": "Di Benua Manakah Letak Pegunungan Andes?", "id": "Amerika Selatan." },
  { "en": "Salvador Dalí Adalah Pelopor Aliran Seni Apa?", "id": "Surealisme." },
  { "en": "Apa Nama Tulang Yang Membentuk Tulang Belakang?", "id": "Vertebra." },
  { "en": "Apa Tujuan Utama Dari Perjanjian Westphalia?", "id": "Mengakhiri Perang Tiga Puluh Tahun." },
  { "en": "Apa Ciri Khas Dari Genre Sastra Distopia?", "id": "Masyarakat Yang Buruk." },
  { "en": "Siapa Astronom Yang Mengemukakan Model Heliosentris?", "id": "Nicolaus Copernicus." },
  { "en": "Apa Nama Ibukota Negara Palau?", "id": "Ngerulmud." },
  { "en": "Talempong Adalah Alat Musik Dari Daerah Mana?", "id": "Minangkabau." },
  { "en": "Apa Istilah Untuk Laju Perubahan Posisi?", "id": "Kecepatan (Velocity)." },
  { "en": "Apa Nama Batas Lubang Hitam Tak Bisa Kembali?", "id": "Horizon Peristiwa." },
  { "en": "Di Benua Manakah Letak Pegunungan Rocky?", "id": "Amerika Utara." },
  { "en": "Claude Monet Adalah Pelopor Aliran Seni Apa?", "id": "Impresionisme." },
  { "en": "Berapa Jumlah Tulang Rusuk Manusia Secara Total?", "id": "24 Tulang (12 Pasang)." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Napoleon?", "id": "Kongres Wina." },
  { "en": "Apa Ciri Khas Dari Genre Sastra Satir?", "id": "Sindiran Atau Ironi." },
  { "en": "Siapa Fisikawan Yang Merumuskan Tiga Hukum Gerak?", "id": "Isaac Newton." },
  { "en": "Apa Nama Ibukota Negara Samoa?", "id": "Apia." },
  { "en": "Kecapi Adalah Alat Musik Dari Daerah Mana?", "id": "Jawa Barat (Sunda)." },
  { "en": "Apa Istilah Untuk Laju Perubahan Kecepatan?", "id": "Percepatan (Akselerasi)." },
  { "en": "Apa Sebutan Untuk Titik Dengan Gravitasi Tak Terhingga?", "id": "Singularitas." },
  { "en": "Pegunungan Apa Yang Memisahkan Eropa Dan Asia?", "id": "Pegunungan Ural." },
  { "en": "Pablo Picasso Adalah Pelopor Aliran Seni Apa?", "id": "Kubisme." },
  { "en": "Apa Nama Tulang Terbesar Di Lengan Bawah?", "id": "Radius Dan Ulna." },
  { "en": "Perjanjian Apa Yang Menyatukan Jerman Pada Abad Ke-19?", "id": "Perjanjian Versailles (1871)." },
  { "en": "Apa Ciri Khas Dari Genre Sastra Epik?", "id": "Kisah Kepahlawanan Panjang." },
  { "en": "Siapa Ilmuwan Yang Terkenal Dengan Eksperimen Loncengnya?", "id": "Robert Boyle." },
  { "en": "Apa Nama Ibukota Kepulauan Solomon?", "id": "Honiara." },
  { "en": "Kolintang Adalah Alat Musik Dari Daerah Mana?", "id": "Minahasa, Sulawesi Utara." },
  { "en": "Apa Besaran Dari Hasil Kali Massa Dan Kecepatan?", "id": "Momentum." },
  { "en": "Apa Nama Titik Terdekat Orbit Satelit Ke Bumi?", "id": "Perigee." },
  { "en": "Di Benua Manakah Letak Pegunungan Atlas?", "id": "Afrika." },
  { "en": "Vincent Van Gogh Diasosiasikan Dengan Aliran Seni Apa?", "id": "Pascaimpresionisme." },
  { "en": "Apa Nama Tulang Dada Di Tengah Rusuk?", "id": "Sternum." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Vietnam?", "id": "Persetujuan Damai Paris." },
  { "en": "Apa Ciri Khas Dari Genre Sastra Tragedi?", "id": "Akhir Cerita Yang Menyedihkan." },
  { "en": "Siapa Ahli Kimia Yang Mengembangkan Tabel Periodik?", "id": "Dmitri Mendeleev." },
  { "en": "Apa Nama Ibukota Negara Federasi Mikronesia?", "id": "Palikir." },
  { "en": "Rebab Adalah Alat Musik Dari Keluarga Apa?", "id": "Alat Musik Gesek." },
  { "en": "Apa Istilah Untuk Perubahan Momentum Suatu Benda?", "id": "Impuls." },
  { "en": "Apa Nama Titik Terjauh Orbit Satelit Dari Bumi?", "id": "Apogee." },
  { "en": "Di Benua Manakah Letak Pegunungan Alpen?", "id": "Eropa." },
  { "en": "Andy Warhol Adalah Tokoh Utama Aliran Seni Apa?", "id": "Seni Pop (Pop Art)." },
  { "en": "Apa Nama Sekelompok Tulang Yang Membentuk Pergelangan Tangan?", "id": "Karpal." },
  { "en": "Perjanjian Apa Yang Membagi Kekaisaran Romawi?", "id": "Tidak Ada Perjanjian Tunggal." },
  { "en": "Apa Ciri Khas Dari Genre Sastra Komedi?", "id": "Menghibur Dan Berakhir Bahagia." },
  { "en": "Siapa Fisikawan Yang Menemukan Hubungan Massa-Energi ($E=mc^2$)?", "id": "Albert Einstein." },
  { "en": "Apa Nama Ibukota Kepulauan Marshall?", "id": "Majuro." },
  { "en": "Bonang Adalah Bagian Dari Ansambel Musik Apa?", "id": "Gamelan." },
  { "en": "Apa Istilah Untuk Gaya Yang Menyebabkan Benda Berputar?", "id": "Torsi." },
  { "en": "Apa Nama Orbit Geostasioner Di Atas Khatulistiwa?", "id": "Sabuk Clarke." },
  { "en": "Di Benua Manakah Letak Pegunungan Himalaya?", "id": "Asia." },
  { "en": "Jackson Pollock Adalah Pelopor Aliran Seni Apa?", "id": "Ekspresionisme Abstrak." },
  { "en": "Apa Nama Sekelompok Tulang Yang Membentuk Telapak Tangan?", "id": "Metakarpal." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Opium Pertama?", "id": "Perjanjian Nanking." },
  { "en": "Apa Ciri Khas Dari Genre Fiksi Ilmiah?", "id": "Berbasis Teknologi Dan Sains." },
  { "en": "Siapa Astronom Yang Memperbaiki Desain Teleskop?", "id": "Galileo Galilei." },
  { "en": "Apa Nama Ibukota Negara Nauru?", "id": "Distrik Yaren (De Facto)." },
  { "en": "Tifa Adalah Alat Musik Dari Daerah Mana?", "id": "Maluku Dan Papua." },
  { "en": "Apa Ukuran Kecenderungan Benda Untuk Terus Bergerak?", "id": "Inersia." },
  { "en": "Apa Nama Orbit Yang Sangat Elips?", "id": "Orbit Molniya." },
  { "en": "Di Benua Manakah Letak Pegunungan Kaukasus?", "id": "Asia Dan Eropa." },
  { "en": "Henri Matisse Adalah Tokoh Utama Aliran Seni Apa?", "id": "Fauvisme." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Tengkorak?", "id": "Kranium." },
  { "en": "Perjanjian Apa Yang Membentuk Uni Eropa Modern?", "id": "Perjanjian Maastricht." },
  { "en": "Apa Ciri Khas Dari Genre Fantasi?", "id": "Unsur Sihir Dan Mitos." },
  { "en": "Siapa Ahli Biologi Yang Terkenal Dengan Sistem Klasifikasinya?", "id": "Carolus Linnaeus." },
  { "en": "Apa Nama Ibukota Negara Tuvalu?", "id": "Funafuti." },
  { "en": "Saron Adalah Bagian Dari Ansambel Musik Apa?", "id": "Gamelan." },
  { "en": "Apa Satuan Standar Internasional Untuk Momentum?", "id": "Kilogram Meter Per Detik." },
  { "en": "Apa Nama Jarak Rata-Rata Bumi Ke Matahari?", "id": "Satuan Astronomi (AU)." },
  { "en": "Di Benua Manakah Letak Pegunungan Appalachia?", "id": "Amerika Utara." },
  { "en": "Edvard Munch Adalah Pelopor Aliran Seni Apa?", "id": "Ekspresionisme." },
  { "en": "Apa Nama Tulang Rahang Bawah Pada Manusia?", "id": "Mandibula." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Revolusi Amerika?", "id": "Perjanjian Paris (1783)." },
  { "en": "Apa Ciri Khas Dari Genre Misteri?", "id": "Teka-Teki Atau Kejahatan." },
  { "en": "Siapa Fisikawan Yang Menemukan Medan Elektromagnetik?", "id": "James Clerk Maxwell." },
  { "en": "Apa Nama Ibukota Negara Kiribati?", "id": "Tarawa Selatan." },
  { "en": "Gendang Adalah Alat Musik Dari Keluarga Apa?", "id": "Perkusi (Membranofon)." },
  { "en": "Apa Istilah Untuk Jumlah Usaha Yang Dilakukan?", "id": "Kerja (Work)." },
  { "en": "Apa Istilah Untuk Titik Terdekat Orbit Ke Matahari?", "id": "Perihelion." },
  { "en": "Di Benua Manakah Letak Pegunungan Drakensberg?", "id": "Afrika." },
  { "en": "Wassily Kandinsky Adalah Pelopor Aliran Seni Apa?", "id": "Seni Abstrak." },
  { "en": "Apa Nama Tulang Yang Melindungi Otak?", "id": "Tulang Tengkorak." },
  { "en": "Perjanjian Apa Yang Membentuk NATO?", "id": "Perjanjian Atlantik Utara." },
  { "en": "Apa Ciri Khas Dari Genre Horor?", "id": "Menimbulkan Rasa Takut." },
  { "en": "Siapa Naturalis Yang Mengusulkan Teori Evolusi?", "id": "Charles Darwin, Alfred Wallace." },
  { "en": "Apa Nama Ibukota Negara Tonga?", "id": "Nukuʻalofa." },
  { "en": "Gong Adalah Alat Musik Dari Keluarga Apa?", "id": "Perkusi (Idiofon)." },
  { "en": "Apa Istilah Untuk Laju Melakukan Usaha?", "id": "Daya (Power)." },
  { "en": "Apa Istilah Untuk Titik Terjauh Orbit Dari Matahari?", "id": "Aphelion." },
  { "en": "Semenanjung Apa Yang Ditempati Oleh Spanyol Dan Portugal?", "id": "Semenanjung Iberia." },
  { "en": "Pada Tahun Berapakah Telepon Pertama Kali Ditemukan?", "id": "Tahun 1876." },
  { "en": "Apa Nama Otot Di Bagian Depan Lengan Atas?", "id": "Bisep." },
  { "en": "Siapa Tsar Rusia Yang Terkenal Memodernisasi Negaranya?", "id": "Pyotr Agung (Peter The Great)." },
  { "en": "Apa Ciri Khas Utama Arsitektur Art Deco?", "id": "Bentuk Geometris Dan Simetris." },
  { "en": "Siapa Filsuf Yunani Yang Menulis 'Nicomachean Ethics'?", "id": "Aristoteles." },
  { "en": "Apa Nama Ibukota Negara Mauritania?", "id": "Nouakchott." },
  { "en": "Seni Membuat Keris Berasal Dari Daerah Mana?", "id": "Jawa, Indonesia." },
  { "en": "Apa Nama Reaksi Kimia Yang Menggabungkan Dua Zat?", "id": "Reaksi Sintesis." },
  { "en": "Apa Penemuan Utama Wahana Antariksa Cassini?", "id": "Detail Cincin Saturnus." },
  { "en": "Semenanjung Apa Yang Mencakup Norwegia Dan Swedia?", "id": "Semenanjung Skandinavia." },
  { "en": "Pada Tahun Berapakah Bola Lampu Pijar Ditemukan?", "id": "Tahun 1879." },
  { "en": "Apa Nama Otot Di Bagian Belakang Lengan Atas?", "id": "Trisep." },
  { "en": "Siapa Ratu Inggris Yang Memerintah Selama Era Victoria?", "id": "Ratu Victoria." },
  { "en": "Apa Ciri Khas Utama Arsitektur Barok?", "id": "Megah, Dramatis, Dan Ornamen." },
  { "en": "Siapa Filsuf Jerman Yang Menulis 'Critique of Pure Reason'?", "id": "Immanuel Kant." },
  { "en": "Apa Nama Ibukota Negara Niger?", "id": "Niamey." },
  { "en": "Seni Tenun Songket Berasal Dari Daerah Mana?", "id": "Sumatra, Indonesia." },
  { "en": "Apa Nama Reaksi Kimia Yang Memecah Satu Senyawa?", "id": "Reaksi Dekomposisi." },
  { "en": "Apa Misi Utama Wahana Antariksa Voyager?", "id": "Menjelajahi Tata Surya Luar." },
  { "en": "Semenanjung Apa Yang Terletak Di Asia Barat Daya?", "id": "Semenanjung Arab." },
  { "en": "Pada Tahun Berapakah Mesin Cetak Pertama Diciptakan?", "id": "Sekitar Tahun 1440." },
  { "en": "Apa Nama Otot Besar Di Bagian Dada?", "id": "Pektoralis Mayor." },
  { "en": "Siapa Kaisar Romawi Pertama Yang Menganut Agama Kristen?", "id": "Konstantinus Agung." },
  { "en": "Apa Ciri Khas Utama Arsitektur Gotik?", "id": "Lengkungan Runcing, Jendela Kaca." },
  { "en": "Siapa Filsuf Perancis Yang Menulis 'The Social Contract'?", "id": "Jean-Jacques Rousseau." },
  { "en": "Apa Nama Ibukota Negara Burkina Faso?", "id": "Ouagadougou." },
  { "en": "Seni Pertunjukan Wayang Kulit Berasal Dari Mana?", "id": "Jawa, Indonesia." },
  { "en": "Apa Nama Reaksi Kimia Yang Melibatkan Oksigen?", "id": "Reaksi Pembakaran (Oksidasi)." },
  { "en": "Apa Penemuan Utama Wahana Antariksa Juno?", "id": "Interior Planet Jupiter." },
  { "en": "Semenanjung Apa Yang Membentuk Bagian Ujung Amerika Selatan?", "id": "Patagonia." },
  { "en": "Pada Tahun Berapakah Pesawat Terbang Pertama Diterbangkan?", "id": "Tahun 1903." },
  { "en": "Apa Nama Kelompok Otot Di Bagian Depan Paha?", "id": "Quadriceps." },
  { "en": "Siapa Firaun Mesir Yang Membangun Piramida Giza?", "id": "Firaun Khufu." },
  { "en": "Apa Ciri Khas Utama Arsitektur Renaisans?", "id": "Simetri, Proporsi, Geometri." },
  { "en": "Siapa Filsuf Tiongkok Yang Mengajarkan Konfusianisme?", "id": "Konfusius." },
  { "en": "Apa Nama Ibukota Negara Somalia?", "id": "Mogadishu." },
  { "en": "Seni Ukir Kayu Jepara Berasal Dari Provinsi Mana?", "id": "Jawa Tengah." },
  { "en": "Apa Nama Reaksi Dimana Asam Dan Basa Bereaksi?", "id": "Reaksi Netralisasi." },
  { "en": "Apa Penemuan Penting Teleskop Luar Angkasa Hubble?", "id": "Memperluas Pandangan Alam Semesta." },
  { "en": "Semenanjung Apa Yang Terletak Di Ujung Tenggara Asia?", "id": "Semenanjung Indochina." },
  { "en": "Pada Tahun Berapakah Mobil Pertama Kali Diciptakan?", "id": "Tahun 1886." },
  { "en": "Apa Nama Otot Besar Di Bagian Punggung Atas?", "id": "Trapezius." },
  { "en": "Siapa Pendiri Kekaisaran Mongol Yang Agung?", "id": "Genghis Khan." },
  { "en": "Apa Ciri Khas Utama Arsitektur Modernis?", "id": "Fungsionalisme Dan Kesederhanaan." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Taoisme?", "id": "Lao Tzu." },
  { "en": "Apa Nama Ibukota Negara Sudan?", "id": "Khartoum." },
  { "en": "Seni Batik Tulis Merupakan Warisan Budaya Dari Mana?", "id": "Indonesia." },
  { "en": "Apa Nama Reaksi Dimana Satu Unsur Menggantikan Lainnya?", "id": "Reaksi Penggantian Tunggal." },
  { "en": "Apa Misi Wahana Antariksa New Horizons?", "id": "Menjelajahi Planet Pluto." },
  { "en": "Semenanjung Apa Yang Menjadi Bagian Dari Yunani?", "id": "Semenanjung Peloponnese." },
  { "en": "Pada Tahun Berapakah Telegraf Pertama Kali Digunakan?", "id": "Tahun 1837." },
  { "en": "Apa Nama Otot Yang Membentuk Dinding Perut?", "id": "Otot Abdomen." },
  { "en": "Siapa Raja Makedonia Yang Menaklukkan Sebagian Besar Dunia?", "id": "Alexander Agung." },
  { "en": "Apa Ciri Khas Arsitektur Klasik Yunani Dan Romawi?", "id": "Penggunaan Kolom Dan Keseimbangan." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep 'Tabula Rasa'?", "id": "John Locke." },
  { "en": "Apa Nama Ibukota Negara Eritrea?", "id": "Asmara." },
  { "en": "Seni Gamelan Adalah Musik Tradisional Dari Mana?", "id": "Jawa Dan Bali, Indonesia." },
  { "en": "Apa Nama Reaksi Dimana Terbentuk Endapan Padat?", "id": "Reaksi Pengendapan." },
  { "en": "Apa Penemuan Utama Wahana Antariksa Magellan?", "id": "Memetakan Permukaan Venus." },
  { "en": "Semenanjung Apa Yang Menjadi Bagian Dari Denmark?", "id": "Semenanjung Jutland." },
  { "en": "Pada Tahun Berapakah Radio Pertama Kali Ditemukan?", "id": "Akhir Abad Ke-19." },
  { "en": "Apa Nama Otot Di Bahu Yang Berbentuk Segitiga?", "id": "Deltoid." },
  { "en": "Siapa Ratu Mesir Kuno Yang Terkenal Dengan Kecantikannya?", "id": "Nefertiti." },
  { "en": "Apa Ciri Khas Utama Arsitektur Brutalisme?", "id": "Material Beton Mentah." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Mazhab Sinisme?", "id": "Diogenes dari Sinope." },
  { "en": "Apa Nama Ibukota Negara Djibouti?", "id": "Kota Djibouti." },
  { "en": "Seni Wayang Orang Berasal Dari Kebudayaan Mana?", "id": "Kebudayaan Jawa." },
  { "en": "Apa Nama Reaksi Yang Menyerap Panas?", "id": "Reaksi Endotermik." },
  { "en": "Apa Misi Utama Wahana Antariksa Rosetta?", "id": "Mendarat Di Sebuah Komet." },
  { "en": "Semenanjung Apa Yang Terletak Di Ujung Selatan Florida?", "id": "Florida Keys." },
  { "en": "Pada Tahun Berapakah Televisi Elektronik Pertama Diciptakan?", "id": "Tahun 1927." },
  { "en": "Apa Nama Otot Betis Di Bagian Belakang Kaki?", "id": "Gastrocnemius." },
  { "en": "Siapa Kaisar Romawi Yang Membagi Kekaisaran Menjadi Dua?", "id": "Diocletian." },
  { "en": "Apa Ciri Khas Utama Arsitektur Art Nouveau?", "id": "Garis Melengkung, Bentuk Organik." },
  { "en": "Siapa Filsuf Yunani Yang Menjadi Guru Alexander Agung?", "id": "Aristoteles." },
  { "en": "Apa Nama Ibukota Negara Gabon?", "id": "Libreville." },
  { "en": "Seni Tari Kecak Berasal Dari Pulau Mana?", "id": "Bali." },
  { "en": "Apa Nama Reaksi Yang Melepaskan Panas?", "id": "Reaksi Eksotermik." },
  { "en": "Apa Penemuan Utama Wahana Antariksa Kepler?", "id": "Ribuan Planet Ekstrasurya." },
  { "en": "Semenanjung Apa Yang Menjadi Bagian Dari Malaysia Dan Thailand?", "id": "Semenanjung Malaya." },
  { "en": "Pada Tahun Berapakah Penisilin Ditemukan?", "id": "Tahun 1928." },
  { "en": "Apa Nama Otot Terpanjang Di Tubuh Manusia?", "id": "Otot Sartorius." },
  { "en": "Siapa Raja Babilonia Yang Membuat Taman Gantung?", "id": "Nebukadnezar II." },
  { "en": "Apa Ciri Khas Utama Arsitektur Postmodern?", "id": "Kombinasi Berbagai Gaya." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Eksistensialisme?", "id": "Søren Kierkegaard." },
  { "en": "Apa Nama Ibukota Negara Guinea?", "id": "Conakry." },
  { "en": "Seni Angklung Merupakan Warisan Budaya Dari Daerah Mana?", "id": "Jawa Barat." },
  { "en": "Apa Nama Reaksi Oksidasi Dan Reduksi?", "id": "Reaksi Redoks." },
  { "en": "Apa Misi Wahana Antariksa Galileo Ke Jupiter?", "id": "Mempelajari Atmosfer Dan Bulan." },
  { "en": "Di Benua Manakah Letak Danau Victoria?", "id": "Afrika." },
  { "en": "Kapan Pertempuran Waterloo Terjadi?", "id": "Tahun 1815." },
  { "en": "Apa Nama Kantung Udara Kecil Di Paru-Paru?", "id": "Alveoli." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Jalan Sutra?", "id": "Kekaisaran Tiongkok (Dinasti Han)." },
  { "en": "Dari Negara Manakah Asal Genre Musik Jazz?", "id": "Amerika Serikat." },
  { "en": "Jane Goodall Adalah Seorang Ahli Dalam Bidang Apa?", "id": "Primatologi." },
  { "en": "Di Negara Manakah Letak Kota Marrakech?", "id": "Maroko." },
  { "en": "Apa Bahan Daging Utama Dalam Masakan Rendang?", "id": "Daging Sapi." },
  { "en": "Zat Dengan PH Kurang Dari 7 Disebut Apa?", "id": "Asam." },
  { "en": "Apa Satuan Jarak Yang Ditempuh Cahaya Setahun?", "id": "Tahun Cahaya." },
  { "en": "Di Benua Manakah Letak Danau Baikal?", "id": "Asia (Siberia)." },
  { "en": "Siapa Yang Memimpin Pasukan Inggris Di Pertempuran Waterloo?", "id": "Duke of Wellington." },
  { "en": "Apa Nama Saluran Udara Utama Menuju Paru-Paru?", "id": "Trakea." },
  { "en": "Kekaisaran Mana Yang Didirikan Oleh Alexander Agung?", "id": "Kekaisaran Makedonia." },
  { "en": "Dari Negara Manakah Asal Genre Musik Reggae?", "id": "Jamaika." },
  { "en": "Dian Fossey Adalah Seorang Ahli Dalam Bidang Apa?", "id": "Gorila (Primatologi)." },
  { "en": "Di Negara Manakah Letak Kota St. Petersburg?", "id": "Rusia." },
  { "en": "Apa Bahan Dasar Utama Masakan Gudeg?", "id": "Nangka Muda." },
  { "en": "Zat Dengan PH Lebih Dari 7 Disebut Apa?", "id": "Basa." },
  { "en": "Apa Satuan Jarak Antarbintang Yang Umum Digunakan?", "id": "Parsec." },
  { "en": "Di Benua Manakah Letak Danau Superior?", "id": "Amerika Utara." },
  { "en": "Kapan Pertempuran Gettysburg Terjadi?", "id": "Tahun 1863." },
  { "en": "Apa Nama Percabangan Trakea Di Paru-Paru?", "id": "Bronkus." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Legiunnya?", "id": "Kekaisaran Romawi." },
  { "en": "Dari Negara Manakah Asal Tarian Tango?", "id": "Argentina." },
  { "en": "Jacques Cousteau Adalah Seorang Ahli Dalam Bidang Apa?", "id": "Biologi Laut." },
  { "en": "Di Negara Manakah Letak Kota Jenewa?", "id": "Swiss." },
  { "en": "Apa Bahan Dasar Utama Masakan Papeda?", "id": "Sagu." },
  { "en": "Kertas Lakmus Merah Berubah Menjadi Biru Dalam Zat Apa?", "id": "Basa." },
  { "en": "Apa Nama Pusat Pelatihan Astronot NASA?", "id": "Johnson Space Center." },
  { "en": "Di Benua Manakah Letak Danau Tanganyika?", "id": "Afrika." },
  { "en": "Siapa Jenderal Yang Kalah Dalam Pertempuran Gettysburg?", "id": "Robert E. Lee." },
  { "en": "Apa Nama Otot Utama Untuk Pernapasan?", "id": "Diafragma." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Pasukan Janissary?", "id": "Kekaisaran Ottoman." },
  { "en": "Dari Negara Manakah Asal Genre Musik Samba?", "id": "Brasil." },
  { "en": "Carl Sagan Adalah Seorang Ahli Dalam Bidang Apa?", "id": "Astronomi." },
  { "en": "Di Negara Manakah Letak Kota Mumbai?", "id": "India." },
  { "en": "Apa Bahan Dasar Utama Masakan Pempek?", "id": "Ikan Dan Sagu." },
  { "en": "Kertas Lakmus Biru Berubah Menjadi Merah Dalam Zat Apa?", "id": "Asam." },
  { "en": "Apa Nama Pusat Peluncuran Roket Utama Rusia?", "id": "Kosmodrom Baikonur." },
  { "en": "Di Benua Manakah Letak Danau Huron?", "id": "Amerika Utara." },
  { "en": "Kapan Pertempuran Hastings Terjadi?", "id": "Tahun 1066." },
  { "en": "Apa Nama Selaput Yang Membungkus Paru-Paru?", "id": "Pleura." },
  { "en": "Kekaisaran Mana Yang Dipimpin Oleh Suleiman Agung?", "id": "Kekaisaran Ottoman." },
  { "en": "Dari Negara Manakah Asal Musik Bossa Nova?", "id": "Brasil." },
  { "en": "Rachel Carson Adalah Seorang Ahli Dalam Bidang Apa?", "id": "Biologi Laut, Konservasi." },
  { "en": "Di Negara Manakah Letak Kota Vancouver?", "id": "Kanada." },
  { "en": "Apa Bahan Dasar Utama Masakan Kerak Telor?", "id": "Beras Ketan Dan Telur." },
  { "en": "Zat Dengan PH Tepat 7 Disebut Apa?", "id": "Netral." },
  { "en": "Apa Nama Pusat Kontrol Misi NASA Di Houston?", "id": "Mission Control Center." },
  { "en": "Di Benua Manakah Letak Danau Michigan?", "id": "Amerika Utara." },
  { "en": "Siapa Pemimpin Normandia Dalam Pertempuran Hastings?", "id": "William Sang Penakluk." },
  { "en": "Apa Nama Ruang Di Antara Dua Paru-Paru?", "id": "Mediastinum." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Sistem Jalan Rayanya?", "id": "Kekaisaran Inka." },
  { "en": "Dari Wilayah Manakah Asal Musik Flamenco?", "id": "Andalusia, Spanyol." },
  { "en": "Stephen Hawking Adalah Ahli Fisika Dalam Bidang Apa?", "id": "Kosmologi Teoretis." },
  { "en": "Di Negara Manakah Letak Kota Dubai?", "id": "Uni Emirat Arab." },
  { "en": "Apa Bahan Dasar Utama Masakan Soto Banjar?", "id": "Ayam Dan Rempah." },
  { "en": "Indikator Universal Berwarna Apa Dalam Larutan Asam?", "id": "Merah Atau Oranye." },
  { "en": "Apa Nama Pusat Peluncuran Utama Eropa?", "id": "Guiana Space Centre." },
  { "en": "Di Benua Manakah Letak Danau Malawi?", "id": "Afrika." },
  { "en": "Kapan Pertempuran Agincourt Terjadi?", "id": "Tahun 1415." },
  { "en": "Apa Nama Tulang Rawan Di Bawah Laring?", "id": "Trakea." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Piramidanya?", "id": "Kekaisaran Mesir Kuno." },
  { "en": "Dari Negara Manakah Asal Musik Fado?", "id": "Portugal." },
  { "en": "Neil deGrasse Tyson Adalah Ahli Fisika Bidang Apa?", "id": "Astrofisika." },
  { "en": "Di Negara Manakah Letak Kota Shanghai?", "id": "Tiongkok." },
  { "en": "Apa Bahan Dasar Utama Masakan Coto Makassar?", "id": "Daging Sapi Dan Jeroan." },
  { "en": "Indikator Universal Berwarna Apa Dalam Larutan Basa?", "id": "Biru Atau Ungu." },
  { "en": "Apa Nama Pusat Peluncuran Roket Milik SpaceX?", "id": "Starbase." },
  { "en": "Di Benua Manakah Letak Great Salt Lake?", "id": "Amerika Utara." },
  { "en": "Perang Apa Yang Diakhiri Oleh Pertempuran Agincourt?", "id": "Perang Seratus Tahun." },
  { "en": "Apa Nama Bagian Atas Tenggorokan?", "id": "Faring." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Taman Gantungnya?", "id": "Kekaisaran Babilonia Baru." },
  { "en": "Dari Negara Manakah Asal Musik Calypso?", "id": "Trinidad Dan Tobago." },
  { "en": "Michio Kaku Adalah Ahli Fisika Bidang Apa?", "id": "Teori String." },
  { "en": "Di Negara Manakah Letak Kota Melbourne?", "id": "Australia." },
  { "en": "Apa Bahan Dasar Utama Masakan Arsik Ikan Mas?", "id": "Ikan Mas Dan Andaliman." },
  { "en": "Apa Angka PH Untuk Air Murni?", "id": "7." },
  { "en": "Apa Nama Pusat Pelatihan Kosmonot Di Rusia?", "id": "Star City." },
  { "en": "Di Benua Manakah Letak Danau Titicaca?", "id": "Amerika Selatan." },
  { "en": "Kapan Pertempuran Trafalgar Terjadi?", "id": "Tahun 1805." },
  { "en": "Apa Nama Kotak Suara Di Dalam Tenggorokan?", "id": "Laring." },
  { "en": "Kekaisaran Mana Yang Dipimpin Oleh Charlemagne?", "id": "Kekaisaran Karoling." },
  { "en": "Dari Negara Manakah Asal Musik Mariachi?", "id": "Meksiko." },
  { "en": "Nikola Tesla Terkenal Karena Karyanya Dalam Bidang Apa?", "id": "Arus Bolak-Balik (AC)." },
  { "en": "Di Negara Manakah Letak Kota Toronto?", "id": "Kanada." },
  { "en": "Apa Bahan Dasar Utama Masakan Ayam Taliwang?", "id": "Ayam Dan Cabai." },
  { "en": "Larutan Apa Yang Digunakan Untuk Menguji Amilum?", "id": "Larutan Iodin." },
  { "en": "Apa Nama Kompleks Peluncuran Utama NASA?", "id": "Kennedy Space Center." },
  { "en": "Di Benua Manakah Letak Laut Kaspia?", "id": "Asia Dan Eropa." },
  { "en": "Siapa Laksamana Inggris Pemenang Pertempuran Trafalgar?", "id": "Horatio Nelson." },
  { "en": "Apa Nama Katup Di Antara Faring Dan Laring?", "id": "Epiglotis." },
  { "en": "Kekaisaran Mana Yang Terkenal Dengan Kode Hukum Hammurabi?", "id": "Kekaisaran Babilonia." },
  { "en": "Dari Negara Manakah Asal Musik K-Pop?", "id": "Korea Selatan." },
  { "en": "Galileo Galilei Adalah Ahli Dalam Bidang Apa?", "id": "Astronomi Observasional." },
  { "en": "Di Negara Manakah Letak Kota Cape Town?", "id": "Afrika Selatan." },
  { "en": "Apa Bahan Dasar Utama Masakan Babi Guling?", "id": "Babi." },
  { "en": "Senyawa Apa Yang Membuat Rasa Asam Pada Lemon?", "id": "Asam Sitrat." },
  { "en": "Apa Nama Stasiun Luar Angkasa Internasional?", "id": "ISS (International Space Station)." },
  { "en": "Teluk Apa Yang Berbatasan Dengan Pantai Selatan Amerika Serikat?", "id": "Teluk Meksiko." },
  { "en": "Dari Negara Manakah Asal Dokumen Magna Carta?", "id": "Inggris." },
  { "en": "Apa Bagian Mata Yang Berfungsi Menerima Cahaya?", "id": "Retina." },
  { "en": "Siapa Yang Bertempur Dalam Perang Krimea Abad Ke-19?", "id": "Rusia Melawan Aliansi Sekutu." },
  { "en": "Apa Istilah Film Untuk Semua Elemen Visual Dalam Adegan?", "id": "Mise-en-scène." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Mesin Pemisah Kapas?", "id": "Eli Whitney." },
  { "en": "Apa Nama Ibukota Negara Montenegro?", "id": "Podgorica." },
  { "en": "Honai Adalah Rumah Adat Khas Dari Provinsi Mana?", "id": "Papua." },
  { "en": "Organel Sel Apa Yang Berfungsi Memodifikasi Protein?", "id": "Aparatus Golgi." },
  { "en": "Apa Benda Langit Berbatu Yang Mengorbit Matahari?", "id": "Asteroid." },
  { "en": "Teluk Persia Terletak Di Antara Benua Apa?", "id": "Asia." },
  { "en": "Apa Dokumen Yang Mendeklarasikan Kemerdekaan Amerika Serikat?", "id": "Deklarasi Kemerdekaan." },
  { "en": "Apa Bagian Telinga Dalam Yang Berbentuk Seperti Siput?", "id": "Koklea." },
  { "en": "Siapa Yang Terlibat Dalam Perang Boer Di Afrika Selatan?", "id": "Inggris Dan Boer." },
  { "en": "Apa Istilah Untuk Seni Menata Pencahayaan Dalam Film?", "id": "Sinematografi." },
  { "en": "Siapakah Yang Menemukan Mesin Uap Yang Efisien?", "id": "James Watt." },
  { "en": "Apa Nama Ibukota Negara Kosovo?", "id": "Pristina." },
  { "en": "Rumah Gadang Adalah Rumah Adat Dari Suku Apa?", "id": "Minangkabau." },
  { "en": "Organel Sel Apa Yang Berfungsi Sebagai Pencerna Seluler?", "id": "Lisosom." },
  { "en": "Apa Benda Langit Ber-Es Yang Memiliki Ekor?", "id": "Komet." },
  { "en": "Teluk Apa Yang Terbesar Di Dunia?", "id": "Teluk Benggala." },
  { "en": "Apa Kumpulan Hukum Kuno Dari Babilonia?", "id": "Kode Hammurabi." },
  { "en": "Apa Reseptor Rasa Di Permukaan Lidah?", "id": "Papila." },
  { "en": "Siapa Yang Bertempur Dalam Perang Rusia-Jepang?", "id": "Kekaisaran Rusia Dan Jepang." },
  { "en": "Apa Istilah Untuk Efek Suara Yang Dibuat Pasca-Produksi?", "id": "Foley." },
  { "en": "Siapakah Yang Menemukan Proses Pasteurisasi?", "id": "Louis Pasteur." },
  { "en": "Apa Nama Ibukota Negara Makedonia Utara?", "id": "Skopje." },
  { "en": "Rumah Joglo Adalah Rumah Adat Dari Daerah Mana?", "id": "Jawa Tengah." },
  { "en": "Organel Sel Apa Yang Menyimpan Air Pada Tumbuhan?", "id": "Vakuola." },
  { "en": "Apa Benda Langit Yang Terbakar Di Atmosfer?", "id": "Meteor." },
  { "en": "Teluk Carpentaria Terletak Di Sebelah Utara Benua Apa?", "id": "Australia." },
  { "en": "Apa Dokumen Yang Mendirikan Perserikatan Bangsa-Bangsa?", "id": "Piagam PBB." },
  { "en": "Apa Bagian Hidung Yang Mendeteksi Bau?", "id": "Epitel Olfaktori." },
  { "en": "Perang Apa Yang Dikenal Sebagai 'Perang Besar'?", "id": "Perang Dunia I." },
  { "en": "Apa Istilah Untuk Proses Menggabungkan Klip Film?", "id": "Editing (Penyuntingan)." },
  { "en": "Siapakah Yang Menemukan Teori Kuman Penyakit?", "id": "Louis Pasteur." },
  { "en": "Apa Nama Ibukota Negara Serbia?", "id": "Beograd." },
  { "en": "Rumah Tongkonan Adalah Rumah Adat Dari Suku Apa?", "id": "Toraja." },
  { "en": "Organel Sel Apa Yang Menjadi Tempat Fotosintesis?", "id": "Kloroplas." },
  { "en": "Apa Sisa Meteor Yang Sampai Ke Permukaan Bumi?", "id": "Meteorit." },
  { "en": "Teluk Fundy Yang Terkenal Dengan Pasang Surutnya Dimana?", "id": "Kanada." },
  { "en": "Apa Dokumen Yang Mengakhiri Perang Dunia I?", "id": "Perjanjian Versailles." },
  { "en": "Apa Reseptor Di Kulit Yang Merasakan Tekanan?", "id": "Mekanosreseptor." },
  { "en": "Siapa Yang Bertempur Dalam Perang Seratus Tahun?", "id": "Inggris Dan Perancis." },
  { "en": "Apa Istilah Untuk Musik Latar Dalam Sebuah Film?", "id": "Skor Film (Soundtrack)." },
  { "en": "Siapakah Yang Menemukan Vaksin Cacar?", "id": "Edward Jenner." },
  { "en": "Apa Nama Ibukota Negara Kroasia?", "id": "Zagreb." },
  { "en": "Lamin Adalah Rumah Adat Khas Suku Apa?", "id": "Suku Dayak." },
  { "en": "Organel Sel Apa Yang Memberi Bentuk Pada Sel Hewan?", "id": "Sitoskeleton." },
  { "en": "Apa Nama Bintang Padat Yang Berputar Sangat Cepat?", "id": "Pulsar." },
  { "en": "Teluk Aden Menghubungkan Laut Merah Dengan Laut Apa?", "id": "Laut Arab." },
  { "en": "Apa Dokumen Yang Membentuk Uni Eropa?", "id": "Perjanjian Maastricht." },
  { "en": "Apa Bagian Telinga Yang Mengubah Getaran Menjadi Sinyal Saraf?", "id": "Koklea." },
  { "en": "Siapa Yang Terlibat Dalam Perang Peloponnesia?", "id": "Athena Dan Sparta." },
  { "en": "Apa Istilah Untuk Sudut Pandang Kamera Dalam Film?", "id": "Point Of View (POV)." },
  { "en": "Siapakah Yang Menemukan Telepon?", "id": "Alexander Graham Bell." },
  { "en": "Apa Nama Ibukota Negara Albania?", "id": "Tirana." },
  { "en": "Sasadu Adalah Rumah Adat Dari Provinsi Mana?", "id": "Maluku Utara." },
  { "en": "Organel Sel Apa Yang Berisi Materi Genetik?", "id": "Nukleus (Inti Sel)." },
  { "en": "Apa Nama Bintang Raksasa Yang Telah Mati?", "id": "Bintang Katai Putih." },
  { "en": "Di Manakah Letak Teluk San Francisco?", "id": "California, Amerika Serikat." },
  { "en": "Apa Nama Dokumen Hak Sipil Di Amerika?", "id": "Civil Rights Act 1964." },
  { "en": "Apa Bagian Mata Yang Mengontrol Ukuran Pupil?", "id": "Iris." },
  { "en": "Siapa Faksi Utama Dalam Perang Saudara Amerika?", "id": "Union Dan Konfederasi." },
  { "en": "Apa Istilah Untuk Gerakan Kamera Secara Horizontal?", "id": "Panning." },
  { "en": "Siapakah Yang Dianggap Sebagai Penemu Radio?", "id": "Guglielmo Marconi." },
  { "en": "Apa Nama Ibukota Negara Slovenia?", "id": "Ljubljana." },
  { "en": "Baileo Adalah Rumah Adat Dari Provinsi Mana?", "id": "Maluku." },
  { "en": "Organel Sel Apa Yang Memproduksi Energi Seluler?", "id": "Mitokondria." },
  { "en": "Apa Objek Astronomi Yang Sangat Terang Dan Jauh?", "id": "Quasar." },
  { "en": "Teluk Hudson Terletak Di Negara Mana?", "id": "Kanada." },
  { "en": "Apa Nama Deklarasi Hak Asasi Manusia Universal?", "id": "Deklarasi Universal Hak Asasi Manusia." },
  { "en": "Apa Bagian Lidah Yang Paling Sensitif Terhadap Rasa Pahit?", "id": "Bagian Belakang." },
  { "en": "Perang Candu Terjadi Antara Negara Mana?", "id": "Inggris Dan Tiongkok." },
  { "en": "Apa Istilah Untuk Gerakan Kamera Secara Vertikal?", "id": "Tilting." },
  { "en": "Siapakah Penemu Bola Lampu Pijar Praktis?", "id": "Thomas Edison." },
  { "en": "Apa Nama Ibukota Negara Bosnia Dan Herzegovina?", "id": "Sarajevo." },
  { "en": "Dalam Loka Adalah Rumah Adat Dari Suku Apa?", "id": "Suku Sumbawa." },
  { "en": "Apa Organel Yang Menjadi Tempat Sintesis Lipid?", "id": "Retikulum Endoplasma Halus." },
  { "en": "Apa Nama Awan Gas Dan Debu Antarbintang?", "id": "Nebula." },
  { "en": "Teluk Thailand Berbatasan Dengan Negara Mana Saja?", "id": "Thailand, Kamboja, Vietnam." },
  { "en": "Apa Nama Dokumen Yang Mengakhiri Apartheid?", "id": "Undang-Undang Diskriminasi Rasial." },
  { "en": "Apa Reseptor Di Kulit Yang Merasakan Suhu?", "id": "Termoreseptor." },
  { "en": "Siapa Yang Terlibat Dalam Perang Tiga Puluh Tahun?", "id": "Protestan Melawan Katolik." },
  { "en": "Apa Istilah Untuk Transisi Adegan Secara Bertahap?", "id": "Dissolve." },
  { "en": "Siapakah Penemu Baterai Listrik?", "id": "Alessandro Volta." },
  { "en": "Apa Nama Ibukota Negara Moldova?", "id": "Chișinău." },
  { "en": "Musalaki Adalah Rumah Adat Dari Provinsi Mana?", "id": "Nusa Tenggara Timur." },
  { "en": "Organel Sel Apa Yang Mensintesis Ribosom?", "id": "Nukleolus." },
  { "en": "Apa Awan Debu Kosmik Yang Menghalangi Cahaya Bintang?", "id": "Nebula Gelap." },
  { "en": "Kanal Apa Yang Menghubungkan Laut Utara Dan Laut Baltik?", "id": "Kanal Kiel." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1932?", "id": "Werner Heisenberg." },
  { "en": "Kelenjar Apa Yang Terletak Di Leher Bagian Depan?", "id": "Kelenjar Tiroid." },
  { "en": "Apa Nama Era Penjelajahan Bangsa Skandinavia?", "id": "Zaman Viking." },
  { "en": "Apa Istilah Untuk Komposisi Musik Tiga Bagian?", "id": "Sonata." },
  { "en": "Sungai Apa Yang Melewati Kota London?", "id": "Sungai Thames." },
  { "en": "Upacara Ngaben Adalah Ritual Dari Daerah Mana?", "id": "Bali." },
  { "en": "Apa Urutan Taksonomi Setelah Kingdom (Kerajaan)?", "id": "Filum (Phylum)." },
  { "en": "Zaman Apa Yang Dikenal Sebagai Zaman Dinosaurus?", "id": "Zaman Mesozoikum." },
  { "en": "Apa Nama Komet Paling Terkenal Di Tata Surya?", "id": "Komet Halley." },
  { "en": "Kanal Apa Yang Menghubungkan Teluk Korintus Dengan Teluk Saronic?", "id": "Kanal Korintus." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1908?", "id": "Ernest Rutherford." },
  { "en": "Kelenjar Apa Yang Memproduksi Hormon Pertumbuhan?", "id": "Kelenjar Pituitari." },
  { "en": "Apa Nama Era Kekuasaan Feodal Di Jepang?", "id": "Zaman Keshogunan." },
  { "en": "Apa Istilah Untuk Lagu Tunggal Dalam Opera?", "id": "Aria." },
  { "en": "Sungai Apa Yang Melewati Kota Paris?", "id": "Sungai Seine." },
  { "en": "Upacara Sekaten Berasal Dari Tradisi Keraton Mana?", "id": "Yogyakarta Dan Surakarta." },
  { "en": "Apa Urutan Taksonomi Setelah Filum?", "id": "Kelas (Class)." },
  { "en": "Zaman Apa Yang Dikenal Sebagai Zaman Mamalia?", "id": "Zaman Kenozoikum." },
  { "en": "Apa Nama Komet Yang Terlihat Pada Tahun 1997?", "id": "Komet Hale-Bopp." },
  { "en": "Tanah Genting Apa Yang Menghubungkan Peloponnese Dengan Yunani?", "id": "Tanah Genting Korintus." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1918?", "id": "Max Planck." },
  { "en": "Kelenjar Apa Yang Menghasilkan Hormon Adrenalin?", "id": "Kelenjar Adrenal." },
  { "en": "Apa Nama Era Pencerahan Di Eropa?", "id": "Abad Pencerahan." },
  { "en": "Apa Istilah Untuk Komposisi Kompleks Dengan Banyak Suara?", "id": "Fugue." },
  { "en": "Sungai Apa Yang Melewati Kota Roma?", "id": "Sungai Tiber." },
  { "en": "Upacara Rambu Solo' Adalah Tradisi Dari Suku Apa?", "id": "Suku Toraja." },
  { "en": "Apa Urutan Taksonomi Setelah Kelas?", "id": "Ordo (Order)." },
  { "en": "Zaman Apa Yang Merupakan Awal Kehidupan Di Bumi?", "id": "Zaman Prakambrium." },
  { "en": "Di Mana Letak Sabuk Asteroid Utama?", "id": "Antara Mars Dan Jupiter." },
  { "en": "Kanal Apa Yang Menghubungkan Great Lakes Ke Samudra Atlantik?", "id": "Saint Lawrence Seaway." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1944?", "id": "Otto Hahn." },
  { "en": "Kelenjar Apa Yang Mengatur Ritme Tidur?", "id": "Kelenjar Pineal." },
  { "en": "Apa Nama Era Ekspansi Imperialisme Eropa?", "id": "Zaman Penjelajahan." },
  { "en": "Apa Istilah Untuk Bagian Pembuka Dalam Musik?", "id": "Overture." },
  { "en": "Sungai Apa Yang Melewati Kota Kairo?", "id": "Sungai Nil." },
  { "en": "Upacara Peusijuek Adalah Tradisi Dari Provinsi Mana?", "id": "Aceh." },
  { "en": "Apa Urutan Taksonomi Setelah Ordo?", "id": "Famili (Family)." },
  { "en": "Pada Zaman Apa Trilobit Banyak Ditemukan?", "id": "Zaman Paleozoikum." },
  { "en": "Apa Nama Asteroid Terbesar Di Sabuk Asteroid?", "id": "Ceres." },
  { "en": "Kanal Apa Yang Merupakan Jalur Air Buatan Terpanjang?", "id": "Kanal Besar Tiongkok." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1923?", "id": "Banting Dan Macleod." },
  { "en": "Kelenjar Apa Yang Mengontrol Kadar Kalsium Darah?", "id": "Kelenjar Paratiroid." },
  { "en": "Apa Nama 'Zaman Kegelapan' Setelah Kejatuhan Roma?", "id": "Abad Pertengahan Awal." },
  { "en": "Apa Istilah Untuk Bagian Akhir Dari Komposisi Musik?", "id": "Coda." },
  { "en": "Sungai Apa Yang Melewati Kota Wina?", "id": "Sungai Danube." },
  { "en": "Tradisi Lompat Batu Berasal Dari Pulau Mana?", "id": "Pulau Nias." },
  { "en": "Apa Urutan Taksonomi Setelah Famili?", "id": "Genus." },
  { "en": "Periode Apa Yang Mengikuti Zaman Mesozoikum?", "id": "Periode Kapur-Tersier." },
  { "en": "Apa Nama Komet Yang Menabrak Jupiter Pada 1994?", "id": "Shoemaker-Levy 9." },
  { "en": "Tanah Genting Apa Yang Terletak Di Ujung Selatan Thailand?", "id": "Tanah Genting Kra." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1954?", "id": "Linus Pauling." },
  { "en": "Kelenjar Apa Yang Memproduksi Hormon Pria?", "id": "Testis." },
  { "en": "Apa Nama Era Perang Antara Athena Dan Sparta?", "id": "Perang Peloponnesia." },
  { "en": "Apa Istilah Untuk Komposisi Musik Bagi Orkestra?", "id": "Simfoni." },
  { "en": "Sungai Apa Yang Melewati Kota Baghdad?", "id": "Sungai Tigris." },
  { "en": "Tradisi Pasola Adalah Ritual Dari Pulau Mana?", "id": "Sumba." },
  { "en": "Apa Urutan Taksonomi Paling Spesifik?", "id": "Spesies (Species)." },
  { "en": "Zaman Apa Yang Ditandai Munculnya Manusia Modern?", "id": "Zaman Holosen." },
  { "en": "Apa Nama Objek Di Luar Neptunus Seperti Pluto?", "id": "Objek Trans-Neptunus." },
  { "en": "Kanal Apa Yang Menghubungkan Samudra Atlantik Dan Pasifik?", "id": "Kanal Panama." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1933?", "id": "Schrödinger Dan Dirac." },
  { "en": "Kelenjar Apa Yang Memproduksi Hormon Wanita?", "id": "Ovarium." },
  { "en": "Apa Nama Era Keemasan Islam Di Spanyol?", "id": "Al-Andalus." },
  { "en": "Apa Istilah Untuk Komposisi Musik Vokal Tanpa Instrumen?", "id": "A cappella." },
  { "en": "Sungai Apa Yang Melewati Kota Bangkok?", "id": "Sungai Chao Phraya." },
  { "en": "Upacara Tiwah Adalah Ritual Dari Suku Mana?", "id": "Suku Dayak." },
  { "en": "Apa Klasifikasi Di Atas Kingdom?", "id": "Domain." },
  { "en": "Zaman Apa Yang Dikenal Sebagai 'Zaman Es'?", "id": "Zaman Pleistosen." },
  { "en": "Apa Nama Komet Yang Berasal Dari Luar Tata Surya?", "id": "Komet Antarbintang." },
  { "en": "Tanah Genting Apa Yang Memisahkan Laut Hitam Dan Azov?", "id": "Tanah Genting Perekop." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1906?", "id": "Golgi Dan Cajal." },
  { "en": "Apa Nama Kelenjar Terbesar Di Tubuh Manusia?", "id": "Hati." },
  { "en": "Apa Nama Era Di Jepang Yang Dipimpin Oleh Shogun?", "id": "Zaman Feodal." },
  { "en": "Apa Istilah Untuk Kecepatan Dalam Musik?", "id": "Tempo." },
  { "en": "Sungai Apa Yang Melewati Kota Budapest?", "id": "Sungai Danube." },
  { "en": "Tradisi Seren Taun Berasal Dari Masyarakat Mana?", "id": "Masyarakat Sunda." },
  { "en": "Siapa Bapak Taksonomi Modern?", "id": "Carolus Linnaeus." },
  { "en": "Zaman Apa Sebelum Periode Dinosaurus?", "id": "Zaman Permian." },
  { "en": "Di Mana Letak Awan Oort?", "id": "Tepi Luar Tata Surya." },
  { "en": "Kanal Apa Yang Terkenal Di Kota Venesia?", "id": "Kanal Besar (Grand Canal)." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1901?", "id": "Wilhelm Röntgen." },
  { "en": "Kelenjar Apa Yang Menghasilkan Air Liur?", "id": "Kelenjar Saliva." },
  { "en": "Apa Nama Era Pembaruan Di Jepang?", "id": "Restorasi Meiji." },
  { "en": "Apa Istilah Untuk Kelompok Instrumen Dalam Orkestra?", "id": "Seksi (Section)." },
  { "en": "Sungai Apa Yang Melewati Kota New Delhi?", "id": "Sungai Yamuna." },
  { "en": "Upacara Kasada Adalah Ritual Dari Suku Mana?", "id": "Suku Tengger." },
  { "en": "Apa Dua Bagian Utama Nama Ilmiah?", "id": "Genus Dan Spesies." },
  { "en": "Zaman Apa Yang Ditandai Ledakan Kehidupan Laut?", "id": "Zaman Kambrium." },
  { "en": "Di Mana Letak Sabuk Kuiper?", "id": "Di Luar Orbit Neptunus." },
  { "en": "Selat Apa Yang Memisahkan Eropa Dan Afrika?", "id": "Selat Gibraltar." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1905?", "id": "Robert Koch." },
  { "en": "Lobus Otak Mana Yang Berfungsi Untuk Berpikir Dan Merencana?", "id": "Lobus Frontal." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Tahun 1812?", "id": "Perjanjian Ghent." },
  { "en": "Siapa Orator Yang Menyampaikan Pidato 'I Have a Dream'?", "id": "Martin Luther King Jr." },
  { "en": "Di Benua Manakah Letak Sungai Amazon?", "id": "Amerika Selatan." },
  { "en": "Musik Gambang Kromong Berasal Dari Kebudayaan Apa?", "id": "Betawi-Tionghoa." },
  { "en": "Apa Nama Ukuran Tinggi Puncak Sebuah Gelombang?", "id": "Amplitudo." },
  { "en": "Apa Tiga Bintang Yang Membentuk Sabuk Orion?", "id": "Alnitak, Alnilam, Mintaka." },
  { "en": "Apa Nama Mata Uang Resmi Negara Jepang?", "id": "Yen." },
  { "en": "Selat Apa Yang Memisahkan Asia Dan Amerika Utara?", "id": "Selat Bering." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1918?", "id": "Fritz Haber." },
  { "en": "Lobus Otak Mana Yang Memproses Informasi Sensorik?", "id": "Lobus Parietal." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Opium Pertama?", "id": "Perjanjian Nanking." },
  { "en": "Siapa Presiden AS Yang Menyampaikan Pidato Gettysburg?", "id": "Abraham Lincoln." },
  { "en": "Di Benua Manakah Letak Sungai Nil?", "id": "Afrika." },
  { "en": "Musik Kroncong Mendapat Pengaruh Dari Negara Mana?", "id": "Portugal." },
  { "en": "Apa Nama Ukuran Jumlah Getaran Per Detik?", "id": "Frekuensi." },
  { "en": "Apa Bintang Paling Terang Di Rasi Bintang Canis Major?", "id": "Sirius." },
  { "en": "Apa Nama Mata Uang Resmi Negara Inggris Raya?", "id": "Pound Sterling." },
  { "en": "Selat Apa Yang Memisahkan Australia Dan Papua Nugini?", "id": "Selat Torres." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1928?", "id": "Charles Nicolle." },
  { "en": "Lobus Otak Mana Yang Berfungsi Untuk Memproses Suara?", "id": "Lobus Temporal." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Tujuh Tahun?", "id": "Perjanjian Paris (1763)." },
  { "en": "Siapa Yang Terkenal Dengan Pidato 'Tear down this wall!'?", "id": "Ronald Reagan." },
  { "en": "Di Benua Manakah Letak Sungai Yangtze?", "id": "Asia." },
  { "en": "Musik Gamelan Degung Berasal Dari Daerah Mana?", "id": "Sunda (Jawa Barat)." },
  { "en": "Apa Nama Jarak Antara Dua Puncak Gelombang?", "id": "Panjang Gelombang." },
  { "en": "Apa Bintang Paling Terang Di Rasi Bintang Lyra?", "id": "Vega." },
  { "en": "Apa Nama Mata Uang Resmi Negara Tiongkok?", "id": "Yuan Renminbi." },
  { "en": "Selat Apa Yang Memisahkan Semenanjung Korea Dan Jepang?", "id": "Selat Korea." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1932?", "id": "Irving Langmuir." },
  { "en": "Lobus Otak Mana Yang Berfungsi Untuk Memproses Penglihatan?", "id": "Lobus Oksipital." },
  { "en": "Perjanjian Apa Yang Membeli Louisiana Dari Perancis?", "id": "Pembelian Louisiana." },
  { "en": "Siapa Yang Terkenal Dengan Pidato 'Never give in'?", "id": "Winston Churchill." },
  { "en": "Di Benua Manakah Letak Sungai Mississippi?", "id": "Amerika Utara." },
  { "en": "Musik Tanjidor Adalah Kesenian Khas Dari Daerah Mana?", "id": "Betawi (Jakarta)." },
  { "en": "Apa Satuan Pengukuran Untuk Frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa Bintang Paling Terang Di Rasi Bintang Aquila?", "id": "Altair." },
  { "en": "Apa Nama Mata Uang Resmi Negara Korea Selatan?", "id": "Won." },
  { "en": "Selat Apa Yang Memisahkan Greenland Dan Islandia?", "id": "Selat Denmark." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1945?", "id": "Fleming, Chain, Florey." },
  { "en": "Apa Bagian Otak Yang Mengatur Emosi Dan Memori?", "id": "Sistem Limbik." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Dunia I?", "id": "Perjanjian Versailles." },
  { "en": "Siapa Yang Terkenal Dengan Pidato 'Give me liberty or give me death!'?", "id": "Patrick Henry." },
  { "en": "Di Benua Manakah Letak Sungai Danube?", "id": "Eropa." },
  { "en": "Musik Angklung Gubrag Berasal Dari Daerah Mana?", "id": "Banten." },
  { "en": "Apa Satuan Pengukuran Untuk Tingkat Kebisingan Suara?", "id": "Desibel (Db)." },
  { "en": "Apa Nama Bintang Utara Yang Menjadi Penunjuk Arah?", "id": "Polaris." },
  { "en": "Apa Nama Mata Uang Resmi Negara Kanada?", "id": "Dolar Kanada." },
  { "en": "Selat Apa Yang Memisahkan Pulau Jawa Dan Sumatra?", "id": "Selat Sunda." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1954?", "id": "Linus Pauling." },
  { "en": "Apa Bagian Otak Yang Berfungsi Sebagai Jembatan Informasi?", "id": "Korpus Kalosum." },
  { "en": "Perjanjian Apa Yang Secara Resmi Membentuk Kanada?", "id": "Akta Konstitusi 1867." },
  { "en": "Siapa Yang Terkenal Dengan Pidato Pengunduran Dirinya?", "id": "Richard Nixon." },
  { "en": "Di Benua Manakah Letak Sungai Mekong?", "id": "Asia." },
  { "en": "Musik Sampek Adalah Kesenian Dari Suku Mana?", "id": "Suku Dayak." },
  { "en": "Apa Istilah Untuk Tinggi Rendahnya Nada Suara?", "id": "Pitch." },
  { "en": "Apa Bintang Paling Terang Di Rasi Bintang Taurus?", "id": "Aldebaran." },
  { "en": "Apa Nama Mata Uang Resmi Negara Australia?", "id": "Dolar Australia." },
  { "en": "Selat Apa Yang Memisahkan Italia Dan Sisilia?", "id": "Selat Messina." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1962?", "id": "Watson, Crick, Wilkins." },
  { "en": "Apa Bagian Otak Yang Mengontrol Rasa Lapar Dan Haus?", "id": "Hipotalamus." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Rusia-Jepang?", "id": "Perjanjian Portsmouth." },
  { "en": "Siapa Yang Terkenal Dengan Pidato 'Blood, toil, tears and sweat'?", "id": "Winston Churchill." },
  { "en": "Di Benua Manakah Letak Sungai Volga?", "id": "Eropa." },
  { "en": "Musik Gong Luang Adalah Kesenian Dari Pulau Mana?", "id": "Bali." },
  { "en": "Apa Kecepatan Suara Di Udara Kering?", "id": "Sekitar 343 Meter Per Detik." },
  { "en": "Apa Bintang Paling Terang Di Rasi Bintang Orion?", "id": "Rigel." },
  { "en": "Apa Nama Mata Uang Resmi Negara Swiss?", "id": "Franc Swiss." },
  { "en": "Selat Apa Yang Menghubungkan Teluk Persia Ke Laut Arab?", "id": "Selat Hormuz." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1964?", "id": "Dorothy Hodgkin." },
  { "en": "Lobus Otak Mana Yang Paling Besar?", "id": "Lobus Frontal." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Spanyol-Amerika?", "id": "Perjanjian Paris (1898)." },
  { "en": "Siapa Yang Terkenal Dengan Pidato Kemerdekaan Indonesia?", "id": "Soekarno." },
  { "en": "Di Benua Manakah Letak Sungai Murray?", "id": "Australia." },
  { "en": "Musik Saluang Adalah Kesenian Dari Daerah Mana?", "id": "Minangkabau." },
  { "en": "Apa Istilah Untuk Gelombang Suara Di Bawah 20 Hz?", "id": "Infrasonik." },
  { "en": "Apa Gugus Bintang Yang Dikenal Sebagai 'Biduk Besar'?", "id": "Ursa Major." },
  { "en": "Apa Nama Mata Uang Resmi Negara India?", "id": "Rupee India." },
  { "en": "Selat Apa Yang Memisahkan Inggris Dan Perancis?", "id": "Selat Inggris (English Channel)." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1983?", "id": "Barbara McClintock." },
  { "en": "Korteks Serebral Merupakan Bagian Dari Otak Mana?", "id": "Otak Besar." },
  { "en": "Perjanjian Apa Yang Menyerahkan Hong Kong Ke Tiongkok?", "id": "Deklarasi Bersama Tiongkok-Inggris." },
  { "en": "Siapa Yang Terkenal Dengan Pidato 'The Ballot or the Bullet'?", "id": "Malcolm X." },
  { "en": "Di Benua Manakah Letak Sungai Kongo?", "id": "Afrika." },
  { "en": "Musik Karinding Berasal Dari Suku Mana?", "id": "Suku Sunda." },
  { "en": "Apa Istilah Untuk Gelombang Suara Di Atas 20.000 Hz?", "id": "Ultrasonik." },
  { "en": "Apa Gugus Bintang Yang Dikenal Sebagai 'Layang-Layang'?", "id": "Crux (Salib Selatan)." },
  { "en": "Apa Nama Mata Uang Resmi Negara Brasil?", "id": "Real Brasil." },
  { "en": "Di Benua Manakah Letak Gurun Kalahari?", "id": "Afrika." },
  { "en": "Siapa Penguasa Rusia Yang Dijuluki 'Yang Mengerikan'?", "id": "Ivan IV Vasilyevich." },
  { "en": "Saluran Apa Yang Menghubungkan Ginjal Ke Kandung Kemih?", "id": "Ureter." },
  { "en": "Dinasti Bourbon Terkenal Memerintah Di Negara Mana?", "id": "Perancis Dan Spanyol." },
  { "en": "Apa Tiga Warna Primer Dalam Cahaya (Aditif)?", "id": "Merah, Hijau, Biru." },
  { "en": "Bandara Internasional Heathrow Terletak Di Kota Mana?", "id": "London." },
  { "en": "Kain Gringsing Berasal Dari Desa Mana Di Bali?", "id": "Desa Tenganan." },
  { "en": "Era Apa Yang Dikenal Sebagai 'Zaman Kehidupan Kuno'?", "id": "Era Paleozoikum." },
  { "en": "Rasi Bintang Apa Yang Terkenal Dengan Bentuk Pemburu?", "id": "Orion." },
  { "en": "Apa Nama Bintang Raksasa Merah Di Rasi Orion?", "id": "Betelgeuse." },
  { "en": "Di Benua Manakah Letak Gurun Great Victoria?", "id": "Australia." },
  { "en": "Siapa Penguasa Utsmaniyah Yang Dijuluki 'Yang Agung'?", "id": "Suleiman I." },
  { "en": "Saluran Apa Yang Mengeluarkan Urin Dari Tubuh?", "id": "Uretra." },
  { "en": "Dinasti Habsburg Terkenal Memerintah Di Wilayah Mana?", "id": "Austria Dan Spanyol." },
  { "en": "Apa Tiga Warna Primer Dalam Cat (Subtraktif)?", "id": "Merah, Kuning, Biru." },
  { "en": "Bandara Internasional Charles de Gaulle Terletak Di Kota Mana?", "id": "Paris." },
  { "en": "Kain Sasirangan Adalah Kerajinan Khas Dari Provinsi Mana?", "id": "Kalimantan Selatan." },
  { "en": "Era Apa Yang Dikenal Sebagai 'Zaman Kehidupan Pertengahan'?", "id": "Era Mesozoikum." },
  { "en": "Rasi Bintang Apa Yang Terkenal Dengan Bentuk Kalajengking?", "id": "Scorpio." },
  { "en": "Apa Nama Bintang Paling Terang Di Rasi Scorpio?", "id": "Antares." },
  { "en": "Di Benua Manakah Letak Gurun Patagonia?", "id": "Amerika Selatan." },
  { "en": "Siapa Penguasa Inggris Yang Dijuluki 'Sang Penakluk'?", "id": "William Sang Penakluk." },
  { "en": "Unit Fungsional Terkecil Di Ginjal Disebut Apa?", "id": "Nefron." },
  { "en": "Dinasti Safawi Adalah Kekaisaran Besar Di Wilayah Mana?", "id": "Persia (Iran)." },
  { "en": "Warna Apa Yang Dihasilkan Dari Campuran Merah Dan Hijau?", "id": "Kuning (Cahaya)." },
  { "en": "Bandara Internasional John F. Kennedy Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Kain Tenun Ikat Adalah Kerajinan Khas Dari Pulau Mana?", "id": "Sumba, Flores." },
  { "en": "Era Apa Yang Dikenal Sebagai 'Zaman Kehidupan Baru'?", "id": "Era Kenozoikum." },
  { "en": "Rasi Bintang Apa Yang Terkenal Dengan Bentuk Singa?", "id": "Leo." },
  { "en": "Apa Nama Bintang Paling Terang Di Rasi Leo?", "id": "Regulus." },
  { "en": "Di Benua Manakah Letak Gurun Kyzylkum?", "id": "Asia Tengah." },
  { "en": "Siapa Penguasa Perancis Yang Dijuluki 'Raja Matahari'?", "id": "Louis XIV." },
  { "en": "Apa Nama Bagian Ginjal Yang Paling Dalam?", "id": "Medula Ginjal." },
  { "en": "Dinasti Ptolemaik Berkuasa Di Kerajaan Kuno Mana?", "id": "Mesir." },
  { "en": "Warna Apa Yang Dihasilkan Dari Campuran Biru Dan Merah?", "id": "Magenta (Cahaya)." },
  { "en": "Bandara Internasional Frankfurt Terletak Di Negara Mana?", "id": "Jerman." },
  { "en": "Kain Besurek Adalah Kain Batik Khas Dari Kota Mana?", "id": "Bengkulu." },
  { "en": "Periode Apa Yang Merupakan Bagian Awal Era Paleozoikum?", "id": "Periode Kambrium." },
  { "en": "Rasi Bintang Apa Yang Terkenal Dengan Bentuk Angsa?", "id": "Cygnus." },
  { "en": "Apa Nama Bintang Paling Terang Di Rasi Cygnus?", "id": "Deneb." },
  { "en": "Di Benua Manakah Letak Gurun Simpson?", "id": "Australia." },
  { "en": "Siapa Penguasa Romawi Yang Dijuluki 'Sang Bijaksana'?", "id": "Marcus Aurelius." },
  { "en": "Apa Nama Bagian Terluar Dari Ginjal?", "id": "Korteks Ginjal." },
  { "en": "Dinasti Maurya Adalah Kekaisaran Kuno Di Negara Mana?", "id": "India." },
  { "en": "Warna Apa Yang Menjadi Pelengkap (Komplementer) Warna Biru?", "id": "Oranye." },
  { "en": "Bandara Internasional Schiphol Terletak Di Dekat Kota Mana?", "id": "Amsterdam." },
  { "en": "Kain Poleng Adalah Kain Motif Khas Dari Pulau Mana?", "id": "Bali." },
  { "en": "Periode Apa Yang Menjadi Akhir Era Dinosaurus?", "id": "Periode Kapur." },
  { "en": "Rasi Bintang Apa Yang Dikenal Sebagai 'Anjing Besar'?", "id": "Canis Major." },
  { "en": "Apa Nama Bintang Paling Terang Di Langit Malam?", "id": "Sirius." },
  { "en": "Di Benua Manakah Letak Gurun Registan?", "id": "Asia." },
  { "en": "Siapa Penguasa Makedonia Ayah Dari Alexander Agung?", "id": "Filipus II dari Makedonia." },
  { "en": "Apa Zat Sisa Utama Yang Disaring Oleh Ginjal?", "id": "Urea." },
  { "en": "Dinasti Tudor Adalah Dinasti Terkenal Dari Negara Mana?", "id": "Inggris." },
  { "en": "Warna Apa Yang Menjadi Pelengkap (Komplementer) Warna Merah?", "id": "Sian (Cyan)." },
  { "en": "Bandara Internasional Dubai Terletak Di Negara Mana?", "id": "Uni Emirat Arab." },
  { "en": "Kain Ulos Adalah Kain Tenun Khas Dari Suku Mana?", "id": "Suku Batak." },
  { "en": "Periode Apa Yang Dikenal Sebagai Zaman Batu Bara?", "id": "Periode Karbon." },
  { "en": "Rasi Bintang Apa Yang Dikenal Sebagai 'Anjing Kecil'?", "id": "Canis Minor." },
  { "en": "Apa Nama Bintang Paling Terang Di Canis Minor?", "id": "Procyon." },
  { "en": "Di Negara Manakah Letak Gurun Danakil?", "id": "Ethiopia." },
  { "en": "Siapa Kaisar Jepang Yang Memimpin Selama Perang Dunia II?", "id": "Kaisar Hirohito." },
  { "en": "Apa Hormon Yang Mengatur Keseimbangan Air Dalam Tubuh?", "id": "Hormon Antidiuretik (ADH)." },
  { "en": "Dinasti Joseon Adalah Dinasti Terakhir Dari Negara Mana?", "id": "Korea." },
  { "en": "Warna Apa Yang Menjadi Pelengkap (Komplementer) Warna Kuning?", "id": "Biru." },
  { "en": "Bandara Internasional Incheon Terletak Di Dekat Kota Mana?", "id": "Seoul." },
  { "en": "Kain Songket Adalah Kain Tenun Khas Dari Daerah Mana?", "id": "Sumatra." },
  { "en": "Pada Periode Apa Manusia Pertama Kali Muncul?", "id": "Periode Kuarter." },
  { "en": "Rasi Bintang Apa Yang Dikenal Sebagai 'Gadis Perawan'?", "id": "Virgo." },
  { "en": "Apa Nama Bintang Paling Terang Di Rasi Virgo?", "id": "Spica." },
  { "en": "Di Benua Manakah Letak Gurun Gibson?", "id": "Australia." },
  { "en": "Siapa Penguasa Kekaisaran Romawi Suci Yang Terkenal?", "id": "Charlemagne." },
  { "en": "Apa Nama Otot Yang Mengontrol Kandung Kemih?", "id": "Otot Sfinkter." },
  { "en": "Dinasti Qing Adalah Dinasti Terakhir Dari Negara Mana?", "id": "Tiongkok." },
  { "en": "Warna-Warna Yang Bersebelahan Dalam Roda Warna Disebut Apa?", "id": "Warna Analog." },
  { "en": "Bandara Internasional Changi Terletak Di Negara Mana?", "id": "Singapura." },
  { "en": "Kerajinan Gerabah Kasongan Berasal Dari Daerah Mana?", "id": "Yogyakarta." },
  { "en": "Periode Apa Yang Terkenal Dengan Banyaknya Amfibi Raksasa?", "id": "Periode Permian." },
  { "en": "Rasi Bintang Apa Yang Memuat Gugus Bintang Pleiades?", "id": "Taurus." },
  { "en": "Apa Nama Bintang Paling Terang Di Rasi Taurus?", "id": "Aldebaran." },
  { "en": "Di Benua Manakah Letak Gurun Thar?", "id": "Asia." },
  { "en": "Siapa Raja Inggris Yang Terkenal Dengan Enam Istrinya?", "id": "Raja Henry VIII." },
  { "en": "Apa Nama Proses Pembentukan Urin Di Ginjal?", "id": "Filtrasi, Reabsorpsi, Augmentasi." },
  { "en": "Dinasti Mughal Berkuasa Di Wilayah Mana?", "id": "Subbenua India." },
  { "en": "Apa Istilah Untuk Kecerahan Atau Kegelapan Suatu Warna?", "id": "Value (Nilai)." },
  { "en": "Bandara Internasional O'Hare Terletak Di Kota Mana?", "id": "Chicago." },
  { "en": "Seni Ukir Asmat Berasal Dari Suku Di Pulau Mana?", "id": "Papua." },
  { "en": "Zaman Apa Yang Dimulai Setelah Kepunahan Dinosaurus?", "id": "Zaman Paleogen." },
  { "en": "Rasi Bintang Apa Yang Menjadi Penunjuk Arah Selatan?", "id": "Crux (Salib Selatan)." },
  { "en": "Apa Nama Bintang Paling Terang Di Rasi Crux?", "id": "Acrux." },
  { "en": "Di Benua Manakah Letak Sungai Mississippi?", "id": "Amerika Utara." },
  { "en": "Winston Churchill Adalah Pemimpin Terkenal Dari Negara Mana?", "id": "Inggris Raya." },
  { "en": "Apa Nama Dua Ruang Atas Jantung?", "id": "Atrium (Serambi)." },
  { "en": "Pada Tahun Berapakah Revolusi Amerika Dimulai?", "id": "Tahun 1775." },
  { "en": "Piramida Giza Terletak Di Dekat Kota Apa?", "id": "Kairo." },
  { "en": "Isaac Newton Dianggap Sebagai Bapak Dari Ilmu Apa?", "id": "Fisika Klasik." },
  { "en": "Gunung Everest Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Himalaya." },
  { "en": "Suling Adalah Jenis Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Bagian Tumbuhan Apa Yang Berfungsi Menyerap Air?", "id": "Akar." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Kecepatan Kapal?", "id": "Knot." },
  { "en": "Di Benua Manakah Letak Sungai Yangtze?", "id": "Asia." },
  { "en": "Nelson Mandela Adalah Pemimpin Terkenal Dari Negara Mana?", "id": "Afrika Selatan." },
  { "en": "Apa Nama Dua Ruang Bawah Jantung?", "id": "Ventrikel (Bilik)." },
  { "en": "Pada Tahun Berapakah Revolusi Rusia Dimulai?", "id": "Tahun 1917." },
  { "en": "Patung Liberty Terletak Di Pelabuhan Kota Mana?", "id": "New York." },
  { "en": "Marie Curie Adalah Perintis Dalam Bidang Apa?", "id": "Radioaktivitas." },
  { "en": "Gunung Kilimanjaro Adalah Puncak Tertinggi Di Benua Mana?", "id": "Afrika." },
  { "en": "Gamelan Sebagian Besar Terdiri Dari Alat Musik Jenis Apa?", "id": "Alat Musik Perkusi." },
  { "en": "Bagian Tumbuhan Apa Yang Menopang Daun Dan Bunga?", "id": "Batang." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Kedalaman Laut?", "id": "Fathom." },
  { "en": "Di Benua Manakah Letak Sungai Amazon?", "id": "Amerika Selatan." },
  { "en": "Mahatma Gandhi Adalah Pemimpin Gerakan Kemerdekaan Negara Mana?", "id": "India." },
  { "en": "Dinding Pemisah Antara Sisi Kiri Dan Kanan Jantung?", "id": "Septum." },
  { "en": "Pada Tahun Berapakah Revolusi Perancis Dimulai?", "id": "Tahun 1789." },
  { "en": "Menara Eiffel Terletak Di Kota Mana?", "id": "Paris." },
  { "en": "Charles Darwin Dikenal Karena Teorinya Dalam Bidang Apa?", "id": "Evolusi." },
  { "en": "Gunung Mont Blanc Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Alpen." },
  { "en": "Angklung Adalah Jenis Alat Musik Apa?", "id": "Idiofon (Perkusi)." },
  { "en": "Bagian Tumbuhan Apa Yang Menjadi Tempat Fotosintesis?", "id": "Daun." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Kekuatan Angin?", "id": "Skala Beaufort." },
  { "en": "Di Benua Manakah Letak Sungai Danube?", "id": "Eropa." },
  { "en": "Abraham Lincoln Adalah Presiden Terkenal Dari Negara Mana?", "id": "Amerika Serikat." },
  { "en": "Katup Apa Yang Terletak Antara Atrium Kanan Dan Ventrikel Kanan?", "id": "Katup Trikuspid." },
  { "en": "Pada Tahun Berapakah Revolusi Kuba Berakhir?", "id": "Tahun 1959." },
  { "en": "Colosseum Adalah Amfiteater Kuno Di Kota Mana?", "id": "Roma." },
  { "en": "Albert Einstein Paling Terkenal Karena Teorinya Tentang Apa?", "id": "Relativitas." },
  { "en": "Gunung Aconcagua Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Andes." },
  { "en": "Sasando Adalah Jenis Alat Musik Apa?", "id": "Alat Musik Petik." },
  { "en": "Bagian Bunga Apa Yang Menghasilkan Serbuk Sari?", "id": "Benang Sari (Stamen)." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Jarak Di Darat?", "id": "Mil." },
  { "en": "Di Benua Manakah Letak Sungai Kongo?", "id": "Afrika." },
  { "en": "Ratu Elizabeth I Adalah Penguasa Terkenal Dari Negara Mana?", "id": "Inggris." },
  { "en": "Katup Apa Yang Terletak Antara Atrium Kiri Dan Ventrikel Kiri?", "id": "Katup Mitral." },
  { "en": "Pada Tahun Berapakah Revolusi Tiongkok (Xinhai) Terjadi?", "id": "Tahun 1911." },
  { "en": "Gedung Opera Sydney Terletak Di Kota Mana?", "id": "Sydney." },
  { "en": "Gregor Mendel Dianggap Sebagai Bapak Dari Ilmu Apa?", "id": "Genetika." },
  { "en": "Denali Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Alaska." },
  { "en": "Rebab Adalah Jenis Alat Musik Apa?", "id": "Alat Musik Gesek." },
  { "en": "Bagian Bunga Apa Yang Mengandung Bakal Biji?", "id": "Putik (Pistil)." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Volume Cairan?", "id": "Galon Atau Liter." },
  { "en": "Di Benua Manakah Letak Sungai Nil?", "id": "Afrika." },
  { "en": "Julius Caesar Adalah Pemimpin Terkenal Dari Peradaban Mana?", "id": "Romawi." },
  { "en": "Pembuluh Arteri Apa Yang Membawa Darah Ke Paru-Paru?", "id": "Arteri Pulmonalis." },
  { "en": "Pada Tahun Berapakah Revolusi Iran Terjadi?", "id": "Tahun 1979." },
  { "en": "Tembok Besar Tiongkok Dibangun Untuk Melindungi Dari Siapa?", "id": "Bangsa Mongol." },
  { "en": "Louis Pasteur Adalah Perintis Dalam Bidang Apa?", "id": "Mikrobiologi." },
  { "en": "Gunung Elbrus Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Kaukasus." },
  { "en": "Kecapi Adalah Jenis Alat Musik Apa?", "id": "Alat Musik Petik." },
  { "en": "Bagian Bunga Apa Yang Melindungi Kuncup?", "id": "Kelopak (Sepal)." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Berat Batu Permata?", "id": "Karat." },
  { "en": "Di Benua Manakah Letak Sungai Volga?", "id": "Eropa." },
  { "en": "Cleopatra Adalah Ratu Terkenal Dari Kerajaan Mana?", "id": "Mesir Ptolemaik." },
  { "en": "Pembuluh Vena Apa Yang Membawa Darah Dari Paru-Paru?", "id": "Vena Pulmonalis." },
  { "en": "Di Negara Mana Revolusi Industri Pertama Kali Dimulai?", "id": "Inggris." },
  { "en": "Taj Mahal Terletak Di Kota Mana?", "id": "Agra." },
  { "en": "Galileo Galilei Adalah Perintis Dalam Bidang Apa?", "id": "Astronomi Observasional." },
  { "en": "Vinson Massif Adalah Puncak Tertinggi Di Benua Mana?", "id": "Antartika." },
  { "en": "Talempong Adalah Jenis Alat Musik Apa?", "id": "Perkusi (Gong Kecil)." },
  { "en": "Bagian Bunga Apa Yang Berwarna Cerah Untuk Menarik Penyerbuk?", "id": "Mahkota (Petal)." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Kertas?", "id": "Rim." },
  { "en": "Di Benua Manakah Letak Sungai Mekong?", "id": "Asia." },
  { "en": "Alexander Agung Adalah Raja Dari Kerajaan Mana?", "id": "Makedonia." },
  { "en": "Apa Nama Pembuluh Arteri Terbesar Dalam Tubuh?", "id": "Aorta." },
  { "en": "Revolusi Apa Yang Terjadi Di Perancis Pada Tahun 1848?", "id": "Revolusi Februari." },
  { "en": "Parthenon Adalah Kuil Kuno Di Kota Mana?", "id": "Athena." },
  { "en": "Nikola Tesla Terkenal Karena Karyanya Dalam Bidang Apa?", "id": "Kelistrikan Arus Bolak-Balik." },
  { "en": "Puncak Jaya Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Sudirman." },
  { "en": "Bonang Adalah Jenis Alat Musik Apa?", "id": "Perkusi (Gong)." },
  { "en": "Proses Penyerbukan Pada Tumbuhan Dibantu Oleh Siapa?", "id": "Angin, Serangga, Hewan." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Tinggi Kuda?", "id": "Hand." },
  { "en": "Di Benua Manakah Letak Sungai Rhein?", "id": "Eropa." },
  { "en": "Napoleon Bonaparte Adalah Kaisar Dari Negara Mana?", "id": "Perancis." },
  { "en": "Apa Nama Alat Pacu Jantung Alami?", "id": "Nodus Sinoatrial." },
  { "en": "Di Negara Mana Revolusi Tenang (Quiet Revolution) Terjadi?", "id": "Kanada (Quebec)." },
  { "en": "Machu Picchu Adalah Situs Kuno Di Negara Mana?", "id": "Peru." },
  { "en": "Rosalind Franklin Berkontribusi Dalam Bidang Apa?", "id": "Struktur DNA." },
  { "en": "Gunung Kosciuszko Adalah Puncak Tertinggi Di Benua Mana?", "id": "Australia." },
  { "en": "Tifa Adalah Jenis Alat Musik Apa?", "id": "Perkusi (Drum)." },
  { "en": "Bagian Tumbuhan Apa Yang Menjadi Alat Reproduksi?", "id": "Bunga." },
  { "en": "Apa Satuan Ukuran Untuk Mengukur Energi Makanan?", "id": "Kalori." },
  { "en": "Semenanjung Sinai Terletak Di Antara Benua Apa?", "id": "Asia Dan Afrika." },
  { "en": "Apa Tujuan Utama Dari 'Ninety-five Theses' Martin Luther?", "id": "Mereformasi Praktik Gereja Katolik." },
  { "en": "Apa Nama Bagian Mata Yang Berfungsi Memfokuskan Cahaya?", "id": "Lensa Mata." },
  { "en": "Ratu Victoria Berasal Dari Dinasti Apa?", "id": "Wangsa Hanover." },
  { "en": "Siapakah Pematung Terkenal Yang Membuat Karya 'Bird in Space'?", "id": "Constantin Brâncuși." },
  { "en": "Hukum Kekekalan Energi Dikenal Sebagai Hukum Termodinamika Ke-?", "id": "Hukum Termodinamika Pertama." },
  { "en": "Di Benua Manakah Letak Gunung Fuji?", "id": "Asia." },
  { "en": "Tari Saman Dari Aceh Memiliki Tujuan Utama Apa?", "id": "Media Dakwah Dan Perayaan." },
  { "en": "Organel Apa Yang Hanya Ditemukan Pada Sel Tumbuhan?", "id": "Dinding Sel Dan Kloroplas." },
  { "en": "Apa Nama Bulan Terbesar Yang Mengorbit Planet Jupiter?", "id": "Ganimede." },
  { "en": "Semenanjung Korea Terletak Di Antara Laut Apa Saja?", "id": "Laut Kuning Dan Laut Jepang." },
  { "en": "Apa Dokumen Yang Menjadi Dasar Hukum Uni Eropa?", "id": "Perjanjian Lisboa." },
  { "en": "Apa Bagian Telinga Yang Berfungsi Menangkap Gelombang Suara?", "id": "Daun Telinga." },
  { "en": "Raja Louis XIV Berasal Dari Dinasti Apa?", "id": "Wangsa Bourbon." },
  { "en": "Siapakah Pematung Yang Membuat 'Venus de Milo'?", "id": "Alexandros dari Antiokhia." },
  { "en": "Hukum Termodinamika Kedua Berhubungan Dengan Konsep Apa?", "id": "Entropi." },
  { "en": "Di Benua Manakah Letak Gunung Kilimanjaro?", "id": "Afrika." },
  { "en": "Tari Piring Dari Minangkabau Melambangkan Apa?", "id": "Rasa Syukur Panen." },
  { "en": "Organel Apa Yang Tidak Ditemukan Pada Sel Tumbuhan?", "id": "Lisosom Dan Sentriol." },
  { "en": "Apa Nama Bulan Terbesar Yang Mengorbit Planet Saturnus?", "id": "Titan." },
  { "en": "Semenanjung Balkan Terletak Di Benua Mana?", "id": "Eropa." },
  { "en": "Apa Tujuan Utama Dari Deklarasi Kemerdekaan Amerika?", "id": "Menyatakan Kemerdekaan Dari Inggris." },
  { "en": "Apa Bagian Hidung Yang Berfungsi Menyaring Udara?", "id": "Bulu Hidung." },
  { "en": "Tsar Nicholas II Berasal Dari Dinasti Apa?", "id": "Wangsa Romanov." },
  { "en": "Siapakah Pematung Yang Membuat Patung 'The Kiss'?", "id": "Auguste Rodin." },
  { "en": "Hukum Termodinamika Ketiga Menyatakan Entropi Mendekati Nol Pada Suhu?", "id": "Nol Absolut." },
  { "en": "Di Benua Manakah Letak Gunung McKinley (Denali)?", "id": "Amerika Utara." },
  { "en": "Tari Pendet Dari Bali Awalnya Berfungsi Sebagai Apa?", "id": "Tarian Persembahan." },
  { "en": "Apa Perbedaan Utama Sel Prokariotik Dan Eukariotik?", "id": "Ada Tidaknya Membran Inti." },
  { "en": "Apa Nama Dua Bulan Yang Mengorbit Planet Mars?", "id": "Fobos Dan Deimos." },
  { "en": "Semenanjung Skandinavia Mencakup Negara Apa Saja?", "id": "Norwegia Dan Swedia." },
  { "en": "Apa Tujuan Utama Dari Piagam Atlantik (Atlantic Charter)?", "id": "Menetapkan Tujuan Perang Sekutu." },
  { "en": "Apa Bagian Lidah Yang Merasakan Manis?", "id": "Ujung Lidah." },
  { "en": "Kaisar Meiji Berasal Dari Dinasti Kekaisaran Mana?", "id": "Wangsa Yamato." },
  { "en": "Siapakah Pematung Yang Membuat Patung 'Ecstasy of Saint Teresa'?", "id": "Gian Lorenzo Bernini." },
  { "en": "Hukum Apa Yang Menyatakan Bahwa Energi Tidak Dapat Diciptakan?", "id": "Hukum Kekekalan Energi." },
  { "en": "Di Benua Manakah Letak Gunung Elbrus?", "id": "Eropa." },
  { "en": "Tari Bedhaya Ketawang Adalah Tarian Sakral Dari Keraton Mana?", "id": "Keraton Surakarta." },
  { "en": "Manakah Yang Memiliki Vakuola Lebih Besar?", "id": "Sel Tumbuhan." },
  { "en": "Apa Nama Bulan Terbesar Yang Mengorbit Planet Neptunus?", "id": "Triton." },
  { "en": "Semenanjung Italia Berbatasan Dengan Laut Apa Saja?", "id": "Laut Mediterania." },
  { "en": "Apa Isi Dari Perjanjian Non-Proliferasi Nuklir?", "id": "Mencegah Penyebaran Senjata Nuklir." },
  { "en": "Apa Bagian Telinga Yang Menjaga Keseimbangan Tekanan Udara?", "id": "Saluran Eustachius." },
  { "en": "Ratu Cleopatra VII Berasal Dari Dinasti Apa?", "id": "Dinasti Ptolemaik." },
  { "en": "Siapakah Pematung Yang Membuat Patung 'Discobolus'?", "id": "Myron." },
  { "en": "Hukum Apa Yang Menjadi Dasar Teori Kinetik Gas?", "id": "Hukum Gas Ideal." },
  { "en": "Di Benua Manakah Letak Pegunungan Alpen?", "id": "Eropa." },
  { "en": "Tari Topeng Cirebon Berasal Dari Provinsi Mana?", "id": "Jawa Barat." },
  { "en": "Apa Nama Dinding Kaku Di Luar Sel Tumbuhan?", "id": "Dinding Sel." },
  { "en": "Apa Nama Bulan Terbesar Kelima Planet Jupiter?", "id": "Amalthea." },
  { "en": "Semenanjung Iberia Mencakup Negara Apa Saja?", "id": "Spanyol Dan Portugal." },
  { "en": "Apa Tujuan Utama Dari Protokol Kyoto?", "id": "Mengurangi Emisi Gas Rumah Kaca." },
  { "en": "Apa Bagian Mata Yang Memberi Warna Pada Mata?", "id": "Iris." },
  { "en": "Sultan Suleiman I Berasal Dari Dinasti Apa?", "id": "Dinasti Utsmaniyah." },
  { "en": "Siapakah Seniman Yang Menciptakan 'Manneken Pis'?", "id": "Jérôme Duquesnoy the Elder." },
  { "en": "Hukum Apa Yang Menyatakan Planet Mengorbit Dalam Elips?", "id": "Hukum Pertama Kepler." },
  { "en": "Di Benua Manakah Letak Gunung Aconcagua?", "id": "Amerika Selatan." },
  { "en": "Tari Remo Adalah Tarian Khas Dari Provinsi Mana?", "id": "Jawa Timur." },
  { "en": "Apa Organel Tempat Terjadinya Fotosintesis?", "id": "Kloroplas." },
  { "en": "Bulan Uranus Manakah Yang Terkenal Dengan Ngarai Raksasa?", "id": "Miranda." },
  { "en": "Semenanjung Arab Terletak Di Benua Mana?", "id": "Asia." },
  { "en": "Apa Dokumen Yang Mengakhiri Perang Saudara Amerika?", "id": "Tidak Ada Perjanjian Formal." },
  { "en": "Apa Nama Sel Fotoreseptor Untuk Penglihatan Malam?", "id": "Sel Batang." },
  { "en": "Kaisar Hirohito Berasal Dari Dinasti Kekaisaran Mana?", "id": "Wangsa Yamato." },
  { "en": "Siapakah Pematung Yang Membuat Gerbang Neraka (The Gates of Hell)?", "id": "Auguste Rodin." },
  { "en": "Hukum Apa Yang Menyatakan Tarikan Antar Dua Benda?", "id": "Hukum Gravitasi Universal Newton." },
  { "en": "Di Benua Manakah Letak Pegunungan Atlas?", "id": "Afrika." },
  { "en": "Tari Kipas Pakarena Berasal Dari Suku Mana?", "id": "Suku Gowa-Makassar." },
  { "en": "Apa Organel Yang Berfungsi Sebagai 'Pabrik Energi' Sel?", "id": "Mitokondria." },
  { "en": "Apa Nama Bulan Saturnus Yang Memiliki Geyser Es?", "id": "Enceladus." },
  { "en": "Semenanjung Indochina Terletak Di Wilayah Mana?", "id": "Asia Tenggara." },
  { "en": "Apa Isi Dari Perjanjian Jenewa?", "id": "Melindungi Korban Perang." },
  { "en": "Apa Nama Sel Fotoreseptor Untuk Penglihatan Warna?", "id": "Sel Kerucut." },
  { "en": "Raja Henry VIII Berasal Dari Dinasti Apa?", "id": "Dinasti Tudor." },
  { "en": "Siapakah Pematung Yang Membuat Patung 'Laocoön and His Sons'?", "id": "Pematung Yunani Kuno." },
  { "en": "Hukum Apa Yang Menghubungkan Tekanan Dan Suhu Gas?", "id": "Hukum Gay-Lussac." },
  { "en": "Di Benua Manakah Letak Pegunungan Rocky?", "id": "Amerika Utara." },
  { "en": "Tari Gambyong Adalah Tarian Klasik Dari Kota Mana?", "id": "Surakarta." },
  { "en": "Organel Apa Yang Mengandung Enzim Pencernaan?", "id": "Lisosom." },
  { "en": "Apa Nama Bulan Jupiter Yang Memiliki Gunung Berapi Aktif?", "id": "Io." },
  { "en": "Semenanjung Malaya Terletak Di Benua Mana?", "id": "Asia." },
  { "en": "Apa Tujuan Utama Dari Perjanjian Paris (2015)?", "id": "Menangani Perubahan Iklim." },
  { "en": "Apa Bagian Telinga Yang Mengandung Saluran Setengah Lingkaran?", "id": "Telinga Dalam." },
  { "en": "Kaisar Augustus Berasal Dari Dinasti Apa?", "id": "Dinasti Julio-Claudian." },
  { "en": "Siapakah Seniman Yang Membuat Patung 'The Little Mermaid'?", "id": "Edvard Eriksen." },
  { "en": "Hukum Apa Yang Menjelaskan Hubungan Volume Dan Suhu Gas?", "id": "Hukum Charles." },
  { "en": "Di Benua Manakah Letak Pegunungan Ural?", "id": "Eropa Dan Asia." },
  { "en": "Tari Jaipong Adalah Kreasi Modern Dari Provinsi Mana?", "id": "Jawa Barat." },
  { "en": "Apa Yang Membedakan Dinding Sel Tumbuhan Dan Jamur?", "id": "Komposisi (Selulosa vs Kitin)." },
  { "en": "Apa Nama Bulan Terbesar Planet Pluto?", "id": "Charon." }



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
