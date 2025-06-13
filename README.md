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


  { "en": "Apa itu dioda?", "id": "Komponen elektronika satu arah." },
  { "en": "Fungsi utama dioda?", "id": "Menyearahkan arus listrik." },
  { "en": "Simbol dioda umum?", "id": "Segitiga dengan garis." },
  { "en": "Apa itu anoda?", "id": "Terminal positif dioda." },
  { "en": "Apa itu katoda?", "id": "Terminal negatif dioda." },
  { "en": "Arah arus dioda?", "id": "Anoda ke katoda." },
  { "en": "Bias maju dioda?", "id": "Anoda positif, katoda negatif." },
  { "en": "Bias mundur dioda?", "id": "Anoda negatif, katoda positif." },
  { "en": "Dioda menyala saat?", "id": "Bias maju." },
  { "en": "Dioda mati saat?", "id": "Bias mundur." },
  { "en": "Tegangan ambang dioda?", "id": "Tegangan mulai konduksi." },
  { "en": "Tegangan ambang silikon?", "id": "Sekitar 0.7 volt." },
  { "en": "Tegangan ambang germanium?", "id": "Sekitar 0.3 volt." },
  { "en": "Bahan semikonduktor dioda?", "id": "Silikon atau germanium." },
  { "en": "Apa itu penyearah?", "id": "Mengubah AC ke DC." },
  { "en": "Dioda penyearah berfungsi?", "id": "Penyearah gelombang." },
  { "en": "Jenis dioda zener?", "id": "Pengatur tegangan." },
  { "en": "Fungsi dioda zener?", "id": "Stabilisasi tegangan." },
  { "en": "Aplikasi dioda zener?", "id": "Regulator tegangan." },
  { "en": "Apa itu LED?", "id": "Dioda pemancar cahaya." },
  { "en": "Fungsi LED?", "id": "Memancarkan cahaya." },
  { "en": "Warna LED ditentukan?", "id": "Material semikonduktor." },
  { "en": "Aplikasi LED?", "id": "Indikator, penerangan." },
  { "en": "Apa itu fotodioda?", "id": "Dioda pendeteksi cahaya." },
  { "en": "Fungsi fotodioda?", "id": "Mengubah cahaya ke listrik." },
  { "en": "Aplikasi fotodioda?", "id": "Sensor cahaya." },
  { "en": "Dioda Schottky?", "id": "Sakelar kecepatan tinggi." },
  { "en": "Keunggulan dioda Schottky?", "id": "Tegangan jatuh rendah." },
  { "en": "Aplikasi dioda Schottky?", "id": "Catu daya switching." },
  { "en": "Apa itu dioda varaktor?", "id": "Dioda kapasitansi variabel." },
  { "en": "Fungsi dioda varaktor?", "id": "Mengubah kapasitansi." },
  { "en": "Aplikasi dioda varaktor?", "id": "Penalaan frekuensi." },
  { "en": "Efek breakdown dioda?", "id": "Kerusakan akibat tegangan." },
  { "en": "Dioda memiliki berapa terminal?", "id": "Dua terminal." },
  { "en": "Apa itu doping?", "id": "Penambahan pengotor semikonduktor." },
  { "en": "Semikonduktor tipe-N?", "id": "Kelebihan elektron." },
  { "en": "Semikonduktor tipe-P?", "id": "Kelebihan hole." },
  { "en": "Apa itu PN junction?", "id": "Pertemuan P dan N." },
  { "en": "Lapisan deplesi dioda?", "id": "Zona tanpa pembawa." },
  { "en": "Arus bocor dioda?", "id": "Arus kecil bias mundur." },
  { "en": "Dioda Bridge Rectifier?", "id": "Penyearah gelombang penuh." },
  { "en": "Kapasitas dioda?", "id": "Kemampuan menahan arus." },
  { "en": "Dioda penyearah daya?", "id": "Untuk arus besar." },
  { "en": "Dioda sinyal kecil?", "id": "Untuk frekuensi tinggi." },
  { "en": "Tegangan maju dioda?", "id": "Tegangan saat konduksi." },
  { "en": "Arus maju dioda?", "id": "Arus saat konduksi." },
  { "en": "Polaritas dioda?", "id": "Arah pemasangan benar." },
  { "en": "Dioda bisa rusak karena?", "id": "Tegangan/arus berlebih." },
  { "en": "Dioda sebagai sakelar?", "id": "On-off sesuai bias." },
  { "en": "Karakteristik IV dioda?", "id": "Grafik arus tegangan." },
  { "en": "Tahan panas dioda?", "id": "Dissipasi daya." },
  { "en": "Jenis dioda umum lainnya?", "id": "Dioda tunnel." },
  { "en": "Apa itu dioda tunnel?", "id": "Dioda resistansi negatif." },
  { "en": "Aplikasi dioda tunnel?", "id": "Osilator frekuensi tinggi." },
  { "en": "Apa itu dioda Gunn?", "id": "Generator gelombang mikro." },
  { "en": "Aplikasi dioda Gunn?", "id": "Radar, osilator." },
  { "en": "Efek Avalanche dioda?", "id": "Arus balik meningkat drastis." },
  { "en": "Dioda penekan transien?", "id": "Melindungi sirkuit." },
  { "en": "Apa itu TVS dioda?", "id": "Dioda penekan tegangan transien." },
  { "en": "Dioda ideal?", "id": "Sakelar sempurna satu arah." },
  { "en": "Dioda dunia nyata?", "id": "Ada rugi tegangan." },
  { "en": "Tingkat pengotor dioda?", "id": "Menentukan karakteristik." },
  { "en": "Panas dioda berasal dari?", "id": "Arus dan resistansi." },
  { "en": "Pendingin dioda digunakan?", "id": "Mencegah overheating." },
  { "en": "Rectifier half-wave?", "id": "Memotong separuh gelombang." },
  { "en": "Rectifier full-wave?", "id": "Menyearahkan semua gelombang." },
  { "en": "Penyebab breakdown zener?", "id": "Tegangan balik berlebih." },
  { "en": "Dioda laser?", "id": "Menghasilkan cahaya laser." },
  { "en": "Aplikasi dioda laser?", "id": "Pembaca barcode, optik." },
  { "en": "Perbedaan dioda vs resistor?", "id": "Arah arus vs hambatan." },
  { "en": "Perbedaan dioda vs kapasitor?", "id": "Penyimpan muatan vs satu arah." },
  { "en": "Dioda pada regulator?", "id": "Penstabil tegangan." },
  { "en": "Arus nominal dioda?", "id": "Arus maksimum aman." },
  { "en": "Tegangan balik puncak?", "id": "Tegangan balik maksimum." },
  { "en": "Pengujian dioda menggunakan?", "id": "Multimeter." },
  { "en": "Dioda rusak tanda?", "id": "Tidak konduksi/short." },
  { "en": "Bagaimana cek dioda?", "id": "Ukur resistansi bolak-balik." },
  { "en": "Resistansi maju dioda?", "id": "Sangat rendah." },
  { "en": "Resistansi mundur dioda?", "id": "Sangat tinggi." },
  { "en": "Dioda sebagai pelindung?", "id": "Melindungi dari polaritas terbalik." },
  { "en": "Sirkuit penjepit dioda?", "id": "Menggeser level sinyal." },
  { "en": "Sirkuit pemotong dioda?", "id": "Membatasi level sinyal." },
  { "en": "Dioda penyearah tegangan?", "id": "Untuk catu daya." },
  { "en": "Dioda sinyal tegangan?", "id": "Untuk sinyal kecil." },
  { "en": "Pemisahan muatan dioda?", "id": "Terjadi di PN junction." },
  { "en": "Aplikasi umum dioda?", "id": "Adaptor daya." },
  { "en": "Dioda pada AC ke DC?", "id": "Penyearah." },
  { "en": "Kehilangan daya dioda?", "id": "Akibat tegangan maju." },
  { "en": "Diode sebagai gerbang logika?", "id": "Bisa untuk gerbang AND/OR." },
  { "en": "Material semikonduktor murni?", "id": "Silikon, germanium." },
  { "en": "Pengotor tipe N?", "id": "Fosfor, Arsen." },
  { "en": "Pengotor tipe P?", "id": "Boron, Gallium." },
  { "en": "Arus dioda bias maju?", "id": "Eksponensial dengan tegangan." },
  { "en": "Dioda pada rangkaian penyearah?", "id": "Elemen penting." },
  { "en": "Dioda daya rendah?", "id": "Untuk sinyal." },
  { "en": "Dioda daya tinggi?", "id": "Untuk penyearah daya." },
  { "en": "Apa itu junction?", "id": "Perbatasan dua material." },
  { "en": "Karakteristik dioda Zener?", "id": "Tegangan breakdown stabil." },
  { "en": "Efek suhu pada dioda?", "id": "Mempengaruhi tegangan maju." },
  { "en": "Dioda pemancar inframerah?", "id": "Untuk remote control." },
  { "en": "Dioda penerima inframerah?", "id": "Untuk sensor." },
  { "en": "Fungsi dasar dioda?", "id": "Mengalirkan arus satu arah." },
  { "en": "Apa itu dioda rectifier?", "id": "Dioda penyearah." },
  { "en": "Apa itu dioda signal?", "id": "Dioda sinyal kecil." },
  { "en": "Dioda SCR artinya?", "id": "Silicon Controlled Rectifier." },
  { "en": "Fungsi utama SCR?", "id": "Sakelar daya." },
  { "en": "Aplikasi SCR?", "id": "Pengendali daya AC." },
  { "en": "Apa itu TRIAC?", "id": "Triode for Alternating Current." },
  { "en": "Fungsi TRIAC?", "id": "Mengontrol daya AC." },
  { "en": "Perbedaan SCR dan TRIAC?", "id": "Satu/dua arah." },
  { "en": "Dioda bridge?", "id": "Empat dioda penyearah." },
  { "en": "Fungsi dioda bridge?", "id": "Penyearah gelombang penuh." },
  { "en": "Apa itu VDR?", "id": "Voltage Dependent Resistor." },
  { "en": "Bagaimana dioda bekerja?", "id": "Hanya satu arah." },
  { "en": "Dioda dalam rangkaian seri?", "id": "Menambah resistansi." },
  { "en": "Dioda dalam rangkaian paralel?", "id": "Membagi arus." },
  { "en": "Arus mundur dioda?", "id": "Sangat kecil." },
  { "en": "Dioda pengaman polaritas?", "id": "Mencegah kerusakan." },
  { "en": "Dioda pada charger?", "id": "Menyearahkan arus." },
  { "en": "Mengapa dioda panas?", "id": "Disipasi daya." },
  { "en": "Tujuan pendingin dioda?", "id": "Menjaga suhu." },
  { "en": "Dioda termistor?", "id": "Sensor suhu." },
  { "en": "Apa itu DIAC?", "id": "Diode for Alternating Current." },
  { "en": "Fungsi DIAC?", "id": "Memicu TRIAC." },
  { "en": "Dioda untuk indikator?", "id": "LED." },
  { "en": "Dioda untuk deteksi?", "id": "Fotodioda." },
  { "en": "Tegangan breakdown Zener?", "id": "Tegangan stabil." },
  { "en": "Dioda sebagai 'check valve'?", "id": "Mengatur aliran." },
  { "en": "Daerah operasi dioda?", "id": "Bias maju dan mundur." },
  { "en": "Dioda ideal vs nyata?", "id": "Tanpa rugi vs ada rugi." },
  { "en": "Karakteristik dioda silikon?", "id": "Vf 0.7V." },
  { "en": "Karakteristik dioda germanium?", "id": "Vf 0.3V." },
  { "en": "Dioda memblokir arus?", "id": "Saat bias mundur." },
  { "en": "Dioda menghantar arus?", "id": "Saat bias maju." },
  { "en": "Arus puncak dioda?", "id": "Arus maksimum sesaat." },
  { "en": "Reverse recovery time dioda?", "id": "Waktu pulih bias mundur." },
  { "en": "Dioda cepat?", "id": "Fast recovery diode." },
  { "en": "Aplikasi dioda cepat?", "id": "Switching frekuensi tinggi." },
  { "en": "Dioda jembatan?", "id": "Bridge rectifier." },
  { "en": "Dioda untuk proteksi?", "id": "Transient Voltage Suppressor." },
  { "en": "Efek suhu pada Vf?", "id": "Vf menurun dengan suhu." },
  { "en": "Dioda dalam SMPS?", "id": "Rectifier, snubber." },
  { "en": "Apa itu freewheeling diode?", "id": "Mencegah tegangan balik." },
  { "en": "Aplikasi freewheeling diode?", "id": "Motor, induktor." },
  { "en": "Dioda untuk bypass?", "id": "Melewatkan arus." },
  { "en": "Dioda untuk clamping?", "id": "Membatasi tegangan." },
  { "en": "Dioda untuk clipping?", "id": "Memotong bagian sinyal." },
  { "en": "Apa itu breakover voltage DIAC?", "id": "Tegangan pemicu." },
  { "en": "Simbol dioda zener?", "id": "Z seperti petir." },
  { "en": "Simbol LED?", "id": "Dioda dengan panah." },
  { "en": "Simbol fotodioda?", "id": "Dioda dengan panah masuk." },
  { "en": "Apa itu reverse current?", "id": "Arus bocor." },
  { "en": "Dioda ideal memiliki?", "id": "Vf nol, Ir nol." },
  { "en": "Faktor daya dioda?", "id": "Kurang dari satu." },
  { "en": "Tingkat kerentanan dioda?", "id": "Terhadap panas, tegangan." },
  { "en": "Dioda daya rendah?", "id": "Arus kurang dari 1A." },
  { "en": "Dioda daya tinggi?", "id": "Arus lebih dari 1A." },
  { "en": "Diode forward current?", "id": "Arus maju." },
  { "en": "Diode reverse voltage?", "id": "Tegangan balik." },
  { "en": "Kapasitansi parasit dioda?", "id": "Kapasitansi internal." },
  { "en": "Dioda kapasitor?", "id": "Varaktor." },
  { "en": "Dioda tegangan rendah?", "id": "Germanium." },
  { "en": "Dioda tegangan tinggi?", "id": "Silikon." },
  { "en": "PN junction dalam dioda?", "id": "Inti dioda." },
  { "en": "Lapisan deplesi melebar?", "id": "Saat bias mundur." },
  { "en": "Lapisan deplesi menyempit?", "id": "Saat bias maju." },
  { "en": "Dioda untuk proteksi induktif?", "id": "Freewheeling diode." },
  { "en": "Dioda pelindung tegangan lebih?", "id": "TVS diode." },
  { "en": "Dioda untuk sirkuit Logika?", "id": "Diode-Transistor Logic." },
  { "en": "Dioda untuk pengatur arus?", "id": "Current-regulating diode." },
  { "en": "Dioda untuk RF?", "id": "Schottky, Varaktor." },
  { "en": "Peran dioda dalam adaptor?", "id": "Penyearahan." },
  { "en": "Dioda untuk detektor sinyal?", "id": "Diode detektor." },
  { "en": "Puncak tegangan balik dioda?", "id": "PIV." },
  { "en": "PIV artinya?", "id": "Peak Inverse Voltage." },
  { "en": "Dioda dapat digunakan sebagai?", "id": "Sakelar, penyearah." },
  { "en": "Dioda dalam rangkaian penjepit?", "id": "Menambah DC offset." },
  { "en": "Dioda dalam rangkaian pemotong?", "id": "Memangkas sinyal." },
  { "en": "Regulator tegangan Zener?", "id": "Menstabilkan tegangan output." },
  { "en": "Dioda pengubah cahaya?", "id": "Fotodioda." },
  { "en": "Dioda pengubah listrik ke cahaya?", "id": "LED." },
  { "en": "Konduktivitas dioda saat panas?", "id": "Meningkat." },
  { "en": "Dioda dengan resistansi negatif?", "id": "Dioda tunnel." },
  { "en": "Dioda dengan dua terminal?", "id": "Semua dioda." },
  { "en": "Efek breakdown dioda Zener?", "id": "Bekerja stabil." },
  { "en": "Efek breakdown umum dioda?", "id": "Kerusakan." },
  { "en": "Dioda pemisah tegangan?", "id": "Voltage divider." },
  { "en": "Dioda pada UPS?", "id": "Rectifier, bypass." },
  { "en": "Dioda pada power supply?", "id": "Penyearah, filter." },
  { "en": "Dioda penggerak LED?", "id": "Current limiting." },
  { "en": "Dioda dengan sambungan logam?", "id": "Schottky." },
  { "en": "Dioda pada sirkuit detektor?", "id": "Mendeteksi puncak." },
  { "en": "Dioda pada sirkuit osilator?", "id": "Varaktor, tunnel." },
  { "en": "Dioda pada sirkuit mixer?", "id": "Mencampur frekuensi." },
  { "en": "Dioda daya rendah sering disebut?", "id": "Dioda sinyal." },
  { "en": "Dioda khusus untuk tegangan rendah?", "id": "Germanium." },
  { "en": "Dioda untuk tegangan tinggi?", "id": "Silicon Carbide." },
  { "en": "Apa itu SiC dioda?", "id": "Silicon Carbide diode." },
  { "en": "Keunggulan SiC dioda?", "id": "Tahan tegangan, suhu tinggi." },
  { "en": "Aplikasi SiC dioda?", "id": "EV, power supply." },
  { "en": "Apa itu breakdown tegangan?", "id": "Tegangan maksimum dioda." },
  { "en": "Dioda berfungsi sebagai switch?", "id": "Ya, dalam bias maju." },
  { "en": "Bagian dioda yang menyala?", "id": "LED." },
  { "en": "Apa itu konduktivitas?", "id": "Kemampuan menghantar listrik." },
  { "en": "Dioda terbuat dari?", "id": "Semikonduktor." },
  { "en": "Arus mengalir melalui anoda?", "id": "Dari anoda ke katoda." },
  { "en": "Penyebab dioda rusak?", "id": "Panas berlebih, arus berlebih." },
  { "en": "Apa itu forward voltage?", "id": "Tegangan ambang." },
  { "en": "Dioda pemancar inframerah?", "id": "IR LED." },
  { "en": "Bagaimana dioda menyearahkan?", "id": "Blokir satu arah." },
  { "en": "Rectifier half wave membutuhkan?", "id": "Satu dioda." },
  { "en": "Rectifier full wave membutuhkan?", "id": "Dua/empat dioda." },
  { "en": "Aplikasi dioda pada radio?", "id": "Detektor sinyal." },
  { "en": "Tegangan dioda Zener tetap?", "id": "Saat breakdown." },
  { "en": "Dioda memblokir arus DC?", "id": "Tidak, hanya AC." },
  { "en": "Apa itu avalanche breakdown?", "id": "Efek penggandaan elektron." },
  { "en": "Dioda penekan tegangan?", "id": "TVS dioda." },
  { "en": "TVS adalah singkatan dari?", "id": "Transient Voltage Suppressor." },
  { "en": "Apa itu schottky barrier?", "id": "Junction logam-semikonduktor." },
  { "en": "Ciri khas dioda Schottky?", "id": "Cepat, Vf rendah." },
  { "en": "Dioda yang bisa berkedip?", "id": "LED." },
  { "en": "Dioda dalam rangkaian sensor?", "id": "Fotodioda." },
  { "en": "Apa itu leakage current?", "id": "Arus bocor dioda." },
  { "en": "Zener diode digunakan untuk?", "id": "Regulator." },
  { "en": "Dioda untuk tuning?", "id": "Varaktor." },
  { "en": "Arus yang kecil di dioda?", "id": "Arus mundur." },
  { "en": "Apa itu reverse bias?", "id": "Blokir arus." },
  { "en": "Forward bias artinya?", "id": "Mengalirkan arus." },
  { "en": "Dioda untuk deteksi panas?", "id": "Termistor." },
  { "en": "Dioda pada panel surya?", "id": "Blocking diode." },
  { "en": "Fungsi blocking diode?", "id": "Mencegah arus balik." },
  { "en": "Apa itu flyback diode?", "id": "Freewheeling diode." },
  { "en": "Dioda jenis laser?", "id": "Laser diode." },
  { "en": "Karakteristik dioda ideal?", "id": "Resistansi nol maju." },
  { "en": "Karakteristik dioda nyata?", "id": "Ada resistansi maju." },
  { "en": "Panas merusak dioda?", "id": "Ya, mengurangi umur." },
  { "en": "Arus maksimum dioda?", "id": "Peringkat arus." },
  { "en": "Dioda sebagai 'one-way valve'?", "id": "Mengatur aliran." },
  { "en": "Diode bridge untuk apa?", "id": "Penyearah gelombang penuh." },
  { "en": "Dioda pada rangkaian inverter?", "id": "Sebagai penyearah." },
  { "en": "Apa itu PIN dioda?", "id": "Dioda kecepatan tinggi." },
  { "en": "Aplikasi PIN dioda?", "id": "RF switch, detektor." },
  { "en": "Dioda untuk cahaya tampak?", "id": "LED." },
  { "en": "Dioda untuk sinar UV?", "id": "UV LED." },
  { "en": "Tegangan minimum LED menyala?", "id": "Forward voltage." },
  { "en": "Efisiensi dioda LED?", "id": "Cahaya per daya." },
  { "en": "Dioda pada power adapter?", "id": "Menyearahkan." },
  { "en": "Resistansi internal dioda?", "id": "Dynamic resistance." },
  { "en": "Dioda untuk proteksi tegangan?", "id": "Zener, TVS." },
  { "en": "Dioda pada catu daya?", "id": "Rectifier, filter." },
  { "en": "Apa itu reverse breakdown?", "id": "Kerusakan saat bias mundur." },
  { "en": "Tegangan jatuh dioda?", "id": "Forward voltage drop." },
  { "en": "Dioda pada rangkaian detektor?", "id": "Diode detektor." },
  { "en": "Dioda sering disebut?", "id": "Penyearah." },
  { "en": "Dioda daya rendah ciri?", "id": "Arus kecil." },
  { "en": "Dioda daya tinggi ciri?", "id": "Arus besar." },
  { "en": "Material semikonduktor dioda?", "id": "Silikon, germanium." },
  { "en": "Pengaruh suhu pada leakage current?", "id": "Meningkat dengan suhu." },
  { "en": "Dioda untuk deteksi radio?", "id": "Diode detektor." },
  { "en": "Dioda pada rangkaian pengali?", "id": "Voltage multiplier." },
  { "en": "Apa itu dopan?", "id": "Zat pengotor." },
  { "en": "Doping meningkatkan?", "id": "Konduktivitas." },
  { "en": "Anoda adalah terminal?", "id": "Positif." },
  { "en": "Katoda adalah terminal?", "id": "Negatif." },
  { "en": "Arah panah simbol dioda?", "id": "Arah arus maju." },
  { "en": "Apa itu blocking voltage?", "id": "Tegangan balik maksimum." },
  { "en": "Dioda untuk pengatur arus?", "id": "Current limiting diode." },
  { "en": "Dioda untuk frekuensi tinggi?", "id": "Schottky, PIN." },
  { "en": "Mengapa dioda penting?", "id": "Menyearahkan, mengatur." },
  { "en": "Apa itu dioda vakum?", "id": "Tabung elektron." },
  { "en": "Fungsi dioda vakum?", "id": "Penyearah, detektor." },
  { "en": "Dioda untuk deteksi radiasi?", "id": "Radiation detector diode." },
  { "en": "Dioda untuk proteksi ESD?", "id": "ESD diode." },
  { "en": "ESD artinya?", "id": "Electrostatic Discharge." },
  { "en": "Dioda untuk sirkuit proteksi?", "id": "Zener, TVS." },
  { "en": "Dioda pada sirkuit switching?", "id": "Schottky, fast recovery." },
  { "en": "Dioda panas jika?", "id": "Arus dan tegangan tinggi." },
  { "en": "Pendingin dioda disebut?", "id": "Heatsink." },
  { "en": "Dioda sebagai gerbang AND?", "id": "Ya, dengan resistor." },
  { "en": "Dioda sebagai gerbang OR?", "id": "Ya, dengan resistor." },
  { "en": "Dioda sering digunakan pada?", "id": "Power supply." },
  { "en": "Diode rectifier daya tinggi?", "id": "Stud mount." },
  { "en": "Apa itu forward current?", "id": "Arus maju." },
  { "en": "Apa itu reverse voltage?", "id": "Tegangan mundur." },
  { "en": "Efek dioda pada sinyal AC?", "id": "Hanya lewati satu setengah." },
  { "en": "Dioda pada sirkuit pengisi daya?", "id": "Sebagai penyearah." },
  { "en": "Dioda sebagai pelindung tegangan?", "id": "Zener, TVS." },
  { "en": "Dioda daya rendah digunakan di?", "id": "Sinyal." },
  { "en": "Dioda daya tinggi digunakan di?", "id": "Catu daya." },
  { "en": "Material dasar dioda?", "id": "Semikonduktor." },
  { "en": "Peran PN junction di dioda?", "id": "Membentuk penghalang." },
  { "en": "Arus bocor dioda terjadi saat?", "id": "Bias mundur." },
  { "en": "Dioda ideal punya tegangan maju?", "id": "Nol volt." },
  { "en": "Dioda nyata punya tegangan maju?", "id": "Lebih dari nol." },
  { "en": "Mengapa dioda penting dalam elektronika?", "id": "Menyearahkan arus." },
  { "en": "Apa itu breakdown Zener?", "id": "Tegangan balik stabil." },
  { "en": "Dioda untuk stabilisasi tegangan?", "id": "Dioda Zener." },
  { "en": "Ciri khas dioda Zener?", "id": "Tegangan breakdown tetap." },
  { "en": "Aplikasi dioda Zener?", "id": "Regulator tegangan." },
  { "en": "Bagaimana cara kerja Zener?", "id": "Bias mundur, breakdown." },
  { "en": "Arus Zener?", "id": "Arus melalui Zener." },
  { "en": "Dioda untuk perlindungan sirkuit?", "id": "TVS dioda." },
  { "en": "TVS berfungsi sebagai?", "id": "Pembatas tegangan." },
  { "en": "Dioda bisa menyearahkan AC?", "id": "Ya, mengubahnya ke DC." },
  { "en": "Apa itu RMS?", "id": "Root Mean Square." },
  { "en": "RMS pada dioda?", "id": "Nilai efektif tegangan/arus." },
  { "en": "Dioda pada charger HP?", "id": "Penyearah, proteksi." },
  { "en": "Mengapa dioda penting?", "id": "Menyearahkan, mengatur arus." },
  { "en": "Apa itu diode drop?", "id": "Penurunan tegangan." },
  { "en": "Tegangan jatuh dioda silikon?", "id": "Sekitar 0.7V." },
  { "en": "Tegangan jatuh dioda germanium?", "id": "Sekitar 0.3V." },
  { "en": "Dioda pada rangkaian detektor radio?", "id": "Detektor AM." },
  { "en": "Apa itu dioda rectifier?", "id": "Dioda penyearah." },
  { "en": "Apa itu dioda sinyal?", "id": "Untuk sinyal kecil." },
  { "en": "Perbedaan fisik anoda dan katoda?", "id": "Cincin di katoda." },
  { "en": "Dioda yang bisa menghasilkan cahaya?", "id": "LED." },
  { "en": "Dioda yang sensitif cahaya?", "id": "Fotodioda." },
  { "en": "Arus bocor dioda akibat?", "id": "Pembawa minoritas." },
  { "en": "Kapasitansi dioda berubah?", "id": "Dengan bias mundur." },
  { "en": "Dioda varaktor mengubah?", "id": "Kapasitansi." },
  { "en": "Aplikasi varaktor?", "id": "Tuning rangkaian." },
  { "en": "Jenis dioda kecepatan tinggi?", "id": "Schottky, PIN." },
  { "en": "Dioda untuk frekuensi radio?", "id": "Schottky, PIN." },
  { "en": "Dioda pada rangkaian osilator?", "id": "Varaktor, tunnel." },
  { "en": "Apa itu breakdown voltage?", "id": "Tegangan balik maksimum." },
  { "en": "Forward current artinya?", "id": "Arus maju." },
  { "en": "Reverse voltage artinya?", "id": "Tegangan mundur." },
  { "en": "Dioda untuk switching?", "id": "Fast recovery diode." },
  { "en": "Mengapa dioda butuh pendingin?", "id": "Dissipasi daya besar." },
  { "en": "Dioda pada UPS digunakan?", "id": "Penyearah, pengatur." },
  { "en": "Karakteristik ideal dioda?", "id": "Resistansi nol, blokir penuh." },
  { "en": "Karakteristik dioda nyata?", "id": "Ada Vf, arus bocor." },
  { "en": "Bagaimana dioda mencegah arus balik?", "id": "Blokir saat bias mundur." },
  { "en": "Dioda pada rangkaian penyearah?", "id": "Komponen utama." },
  { "en": "Apa itu blocking diode?", "id": "Mencegah arus balik." },
  { "en": "Aplikasi blocking diode?", "id": "Panel surya." },
  { "en": "Dioda yang bisa berkedip?", "id": "Blinking LED." },
  { "en": "Apa itu photo diode?", "id": "Dioda fotosensitif." },
  { "en": "Fungsi photo diode?", "id": "Detektor cahaya." },
  { "en": "Dioda jenis IR?", "id": "Infrared LED." },
  { "en": "Aplikasi IR LED?", "id": "Remote control." },
  { "en": "Dioda untuk sinar UV?", "id": "UV LED." },
  { "en": "Apa itu current limiting diode?", "id": "Mengatur arus." },
  { "en": "Dioda pada catu daya?", "id": "Menyearahkan." },
  { "en": "Dioda pada sirkuit perlindungan?", "id": "TVS, Zener." },
  { "en": "Dioda pada sirkuit penjepit?", "id": "Pembatas tegangan." },
  { "en": "Apa itu flyback diode?", "id": "Proteksi tegangan induktif." },
  { "en": "Dioda untuk konversi daya?", "id": "Rectifier." },
  { "en": "Apa itu diffused junction?", "id": "Metode pembuatan dioda." },
  { "en": "Apa itu alloy junction?", "id": "Metode pembuatan dioda." },
  { "en": "Dioda memiliki daerah deplesi?", "id": "Ya, lapisan kosong." },
  { "en": "Dioda sebagai pengalih?", "id": "Switching." },
  { "en": "Dioda daya rendah?", "id": "Sinyal kecil." },
  { "en": "Dioda daya tinggi?", "id": "Aplikasi daya." },
  { "en": "Efek suhu pada tegangan maju?", "id": "Menurun." },
  { "en": "Efek suhu pada arus bocor?", "id": "Meningkat." },
  { "en": "Dioda untuk sirkuit detektor?", "id": "Detektor AM/FM." },
  { "en": "Dioda untuk sirkuit pemotong?", "id": "Memotong bagian sinyal." },
  { "en": "Apa itu dioda SCR?", "id": "Sakelar daya." },
  { "en": "Apa itu dioda TRIAC?", "id": "Kontroler daya AC." },
  { "en": "Dioda untuk pengatur waktu?", "id": "Menggunakan RC." },
  { "en": "Dioda pada rangkaian gerbang?", "id": "AND, OR." },
  { "en": "Apa itu doping tipe-N?", "id": "Kelebihan elektron." },
  { "en": "Apa itu doping tipe-P?", "id": "Kelebihan hole." },
  { "en": "Material dasar semikonduktor?", "id": "Silikon, Germanium." },
  { "en": "Karakteristik IV dioda?", "id": "Arus vs Tegangan." },
  { "en": "Apa itu reverse recovery time?", "id": "Waktu pulih bias mundur." },
  { "en": "Dioda cepat diperlukan untuk?", "id": "Switching cepat." },
  { "en": "Dioda untuk deteksi panas?", "id": "Thermistor." },
  { "en": "Apa itu rectifier?", "id": "Penyearah." },
  { "en": "Tujuan utama rectifier?", "id": "Mengubah AC ke DC." },
  { "en": "Dioda jembatan untuk?", "id": "Penyearah gelombang penuh." },
  { "en": "Dioda dalam rangkaian penerangan?", "id": "LED." },
  { "en": "Dioda pada sistem fotovoltaik?", "id": "Blocking, bypass." },
  { "en": "Fungsi bypass diode?", "id": "Melindungi panel surya." },
  { "en": "Apa itu breakdown region?", "id": "Daerah breakdown Zener." },
  { "en": "Dioda pada remote control?", "id": "Infrared LED." },
  { "en": "Dioda pada sensor optik?", "id": "Fotodioda." },
  { "en": "Perbedaan LED dan dioda biasa?", "id": "Emisi cahaya." },
  { "en": "Dioda untuk penstabil tegangan?", "id": "Zener." },
  { "en": "Apa itu tunnel diode?", "id": "Dioda resistansi negatif." },
  { "en": "Aplikasi tunnel diode?", "id": "Osilator frekuensi tinggi." },
  { "en": "Dioda Gunn untuk?", "id": "Generator gelombang mikro." },
  { "en": "Dioda sebagai pengubah level?", "id": "Level shifter." },
  { "en": "Dioda untuk mencegah short?", "id": "Proteksi." },
  { "en": "Apa itu PIV rating?", "id": "Tegangan balik puncak." },
  { "en": "PIV penting untuk?", "id": "Pemilihan dioda." },
  { "en": "Dioda yang mengeluarkan cahaya?", "id": "Light-Emitting Diode." },
  { "en": "Apa itu breakdown tegangan Zener?", "id": "Tegangan stabil dioda Zener." },
  { "en": "Dioda untuk regulasi tegangan?", "id": "Dioda Zener." },
  { "en": "Apa fungsi utama dioda Zener?", "id": "Menstabilkan tegangan." },
  { "en": "Di mana Zener sering digunakan?", "id": "Regulator daya." },
  { "en": "Bagaimana Zener dihubungkan?", "id": "Bias mundur." },
  { "en": "Apa itu arus Zener?", "id": "Arus saat breakdown." },
  { "en": "Dioda penekan lonjakan tegangan?", "id": "TVS Diode." },
  { "en": "TVS melindungi dari?", "id": "Tegangan transien." },
  { "en": "Dioda mengubah AC menjadi?", "id": "DC." },
  { "en": "Apa itu Ripple Factor?", "id": "Derajat kehalusan DC." },
  { "en": "Dioda pada pengisi daya baterai?", "id": "Penyearah." },
  { "en": "Mengapa dioda penting dalam elektronik?", "id": "Mengatur aliran arus." },
  { "en": "Apa itu tegangan jatuh maju?", "id": "Kehilangan tegangan dioda." },
  { "en": "Tegangan jatuh Schottky?", "id": "Sangat rendah." },
  { "en": "Dioda pada sirkuit detektor AM?", "id": "Menghilangkan pembawa." },
  { "en": "Apa itu dioda umum?", "id": "Dioda penyearah." },
  { "en": "Apa itu dioda switching?", "id": "Dioda kecepatan tinggi." },
  { "en": "Anoda ditandai dengan?", "id": "Tidak ada tanda khusus." },
  { "en": "Katoda ditandai dengan?", "id": "Garis atau cincin." },
  { "en": "Dioda yang memancarkan warna?", "id": "LED." },
  { "en": "Dioda yang mengubah cahaya?", "id": "Fotodioda." },
  { "en": "Arus bocor dioda terjadi di?", "id": "Daerah deplesi." },
  { "en": "Kapasitansi dioda dimanfaatkan di?", "id": "Dioda varaktor." },
  { "en": "Fungsi varaktor?", "id": "Variabel kapasitansi." },
  { "en": "Aplikasi varaktor?", "id": "Penalaan frekuensi." },
  { "en": "Dioda dengan respon cepat?", "id": "Schottky." },
  { "en": "Dioda untuk frekuensi tinggi (HF)?", "id": "Schottky." },
  { "en": "Dioda pada sirkuit osilator?", "id": "Tunnel diode." },
  { "en": "Apa itu tegangan puncak terbalik?", "id": "Reverse peak voltage." },
  { "en": "Arus maju dioda?", "id": "Arus saat konduksi." },
  { "en": "Tegangan mundur dioda?", "id": "Tegangan saat blokir." },
  { "en": "Dioda untuk pemulihan cepat?", "id": "Fast recovery diode." },
  { "en": "Mengapa dioda panas saat bekerja?", "id": "Disipasi daya." },
  { "en": "Pendingin dioda disebut juga?", "id": "Heat sink." },
  { "en": "Dioda pada catu daya komputer?", "id": "Rectifier." },
  { "en": "Apa itu dioda ideal?", "id": "Tidak ada rugi daya." },
  { "en": "Apa itu dioda nyata?", "id": "Ada rugi daya." },
  { "en": "Bagaimana dioda menjaga satu arah?", "id": "Lapisan deplesi." },
  { "en": "Dioda pada panel surya?", "id": "Bypass dan blocking." },
  { "en": "Fungsi bypass dioda?", "id": "Lewati panel teduh." },
  { "en": "Dioda yang berkedip sendiri?", "id": "Blinking LED." },
  { "en": "Apa itu photodioda?", "id": "Dioda sensitif cahaya." },
  { "en": "Aplikasi photodioda?", "id": "Sensor cahaya." },
  { "en": "Dioda untuk transmisi IR?", "id": "IR LED." },
  { "en": "Aplikasi UV LED?", "id": "Sterilisasi." },
  { "en": "Apa itu diode current regulator?", "id": "Dioda pengatur arus." },
  { "en": "Dioda pada power supply?", "id": "Penyearah, regulator." },
  { "en": "Dioda pada sirkuit proteksi tegangan?", "id": "Zener, TVS." },
  { "en": "Dioda pada sirkuit clipper?", "id": "Memotong sinyal." },
  { "en": "Apa itu flyback dioda?", "id": "Pelindung tegangan balik." },
  { "en": "Dioda untuk konversi energi?", "id": "Penyearah." },
  { "en": "Metode pembuatan dioda?", "id": "Diffused junction." },
  { "en": "Dioda memiliki daerah PN?", "id": "Ya." },
  { "en": "Dioda sebagai sakelar elektronik?", "id": "Ya." },
  { "en": "Dioda daya rendah untuk?", "id": "Sinyal." },
  { "en": "Dioda daya tinggi untuk?", "id": "Daya." },
  { "en": "Pengaruh suhu pada Vf?", "id": "Menurun." },
  { "en": "Pengaruh suhu pada Ir?", "id": "Meningkat." },
  { "en": "Dioda pada sirkuit detektor?", "id": "Detektor radio." },
  { "en": "Dioda pada sirkuit limiter?", "id": "Pembatas." },
  { "en": "Apa itu SCR?", "id": "Silicon Controlled Rectifier." },
  { "en": "Apa itu TRIAC?", "id": "Triode for Alternating Current." },
  { "en": "Dioda untuk pengatur waktu?", "id": "Rangkaian timing." },
  { "en": "Dioda pada gerbang logika?", "id": "AND/OR." },
  { "en": "Pengotor tipe-N memberikan?", "id": "Elektron bebas." },
  { "en": "Pengotor tipe-P memberikan?", "id": "Hole." },
  { "en": "Bahan dasar semikonduktor?", "id": "Silikon." },
  { "en": "Kurva IV dioda menunjukkan?", "id": "Karakteristik arus tegangan." },
  { "en": "Waktu pemulihan mundur?", "id": "Reverse recovery time." },
  { "en": "Dioda cepat untuk?", "id": "Switching." },
  { "en": "Dioda untuk sensor suhu?", "id": "Termistor." },
  { "en": "Apa itu rectifier bridge?", "id": "Penyearah gelombang penuh." },
  { "en": "Dioda pada penerangan rumah?", "id": "LED." },
  { "en": "Dioda pada sirkuit fotovoltaik?", "id": "Blocking dan bypass." },
  { "en": "Dioda sebagai pengatur level?", "id": "Level shifter." },
  { "en": "Dioda mencegah sirkuit pendek?", "id": "Melindungi beban." },
  { "en": "Apa itu forward resistance?", "id": "Resistansi saat konduksi." },
  { "en": "Apa itu reverse resistance?", "id": "Resistansi saat blokir." },
  { "en": "Dioda pada charger ponsel?", "id": "Menyearahkan." },
  { "en": "Dioda untuk proteksi tegangan lebih?", "id": "Zener, TVS." },
  { "en": "Dioda sinyal digunakan di?", "id": "Elektronik kecil." },
  { "en": "Dioda daya digunakan di?", "id": "Aplikasi daya." },
  { "en": "Bagaimana doping memengaruhi dioda?", "id": "Konduktivitas." },
  { "en": "Apa itu potensial barrier?", "id": "Penghalang tegangan." },
  { "en": "Potensial barrier dioda silikon?", "id": "0.7 volt." },
  { "en": "Dioda ideal punya arus bocor?", "id": "Nol." },
  { "en": "Dioda nyata punya arus bocor?", "id": "Ada." },
  { "en": "Mengapa dioda penting di sirkuit?", "id": "Kontrol aliran arus." },
  { "en": "Diode untuk proteksi ESD?", "id": "TVS dioda." },
  { "en": "Dioda untuk sirkuit switching?", "id": "Schottky." },
  { "en": "Dioda memancarkan panas?", "id": "Ya, saat bekerja." },
  { "en": "Dioda dapat berfungsi sebagai?", "id": "Penyearah, sakelar." },
  { "en": "Dioda pengubah frekuensi?", "id": "Varaktor." },
  { "en": "Apa itu tegangan Zener?", "id": "Tegangan stabil bias mundur." },
  { "en": "Bagaimana Zener digunakan?", "id": "Sebagai regulator tegangan." },
  { "en": "Apa itu reverse breakdown Zener?", "id": "Daerah operasi stabil." },
  { "en": "Dioda untuk perlindungan lonjakan?", "id": "TVS." },
  { "en": "TVS dioda melindungi dari?", "id": "Tegangan transien tinggi." },
  { "en": "Dioda menyearahkan arus?", "id": "AC ke DC." },
  { "en": "Apa itu arus ripple?", "id": "Komponen AC pada DC." },
  { "en": "Dioda pada sistem audio?", "id": "Rectifier." },
  { "en": "Mengapa dioda itu penting?", "id": "Fungsi dasar elektronika." },
  { "en": "Apa itu forward voltage drop?", "id": "Penurunan tegangan maju." },
  { "en": "Vf dioda Schottky?", "id": "Sangat rendah." },
  { "en": "Dioda pada sirkuit detektor FM?", "id": "Tidak umum." },
  { "en": "Apa itu dioda umum?", "id": "1N4001." },
  { "en": "Apa itu dioda switching?", "id": "1N4148." },
  { "en": "Bagian anoda dioda?", "id": "Terminal positif." },
  { "en": "Bagian katoda dioda?", "id": "Terminal negatif." },
  { "en": "Dioda yang memancarkan cahaya?", "id": "Light Emitting Diode." },
  { "en": "Dioda yang mendeteksi cahaya?", "id": "Photodiode." },
  { "en": "Arus bocor dioda dipengaruhi?", "id": "Suhu." },
  { "en": "Kapasitansi dioda digunakan untuk?", "id": "Variabel frekuensi." },
  { "en": "Dioda varaktor mengubah apa?", "id": "Kapasitansi." },
  { "en": "Aplikasi varaktor?", "id": "Tuning radio." },
  { "en": "Dioda dengan kecepatan paling tinggi?", "id": "Schottky, PIN." },
  { "en": "Dioda untuk frekuensi mikro?", "id": "PIN diode." },
  { "en": "Dioda pada sirkuit penguat?", "id": "Bias." },
  { "en": "Apa itu PIV?", "id": "Peak Inverse Voltage." },
  { "en": "Arus maju nominal?", "id": "Arus kerja aman." },
  { "en": "Tegangan mundur maksimum?", "id": "Peringkat tegangan." },
  { "en": "Dioda pemulihan cepat untuk?", "id": "Switching daya." },
  { "en": "Mengapa dioda menghasilkan panas?", "id": "Karena resistansi internal." },
  { "en": "Dioda pada catu daya linier?", "id": "Rectifier." },
  { "en": "Dioda ideal vs nyata?", "id": "Perilaku berbeda." },
  { "en": "Bagaimana dioda mengontrol arus?", "id": "Memblokir arah." },
  { "en": "Dioda untuk perlindungan polaritas?", "id": "Mencegah terbalik." },
  { "en": "Apa itu bypass diode?", "id": "Melindungi panel PV." },
  { "en": "Dioda yang bisa berkedip?", "id": "Blinking LED." },
  { "en": "Apa itu detektor cahaya?", "id": "Fotodioda." },
  { "en": "Aplikasi fotodioda?", "id": "Sensor." },
  { "en": "Dioda untuk pengiriman IR?", "id": "Infrared LED." },
  { "en": "Aplikasi IR LED?", "id": "Remote TV." },
  { "en": "Dioda untuk sinar UV?", "id": "UV LED." },
  { "en": "Apa itu CLD?", "id": "Current Limiting Diode." },
  { "en": "Dioda pada power supply?", "id": "Penyearah, regulator." },
  { "en": "Dioda pada sirkuit pembatas?", "id": "Menjepit sinyal." },
  { "en": "Dioda untuk proteksi induktif?", "id": "Flyback diode." },
  { "en": "Dioda untuk konversi tegangan?", "id": "Voltage multiplier." },
  { "en": "Metode pembuatan dioda?", "id": "Epitaxial." },
  { "en": "Dioda memiliki daerah deplesi?", "id": "Antara P dan N." },
  { "en": "Dioda sebagai sakelar?", "id": "On/off." },
  { "en": "Dioda daya rendah?", "id": "Sinyal." },
  { "en": "Dioda daya tinggi?", "id": "Rektifikasi." },
  { "en": "Pengaruh suhu pada kinerja dioda?", "id": "Perubahan karakteristik." },
  { "en": "Dioda pada sirkuit detektor gelombang?", "id": "Detektor sinyal." },
  { "en": "Dioda pada sirkuit clipper?", "id": "Memotong bagian gelombang." },
  { "en": "Apa itu Silicon Controlled Rectifier?", "id": "SCR." },
  { "en": "Apa itu DIAC?", "id": "Diode for AC." },
  { "en": "Dioda untuk pengatur waktu?", "id": "Rangkaian RC." },
  { "en": "Dioda pada gerbang logika?", "id": "AND/OR." },
  { "en": "Doping tipe-N artinya?", "id": "Kelebihan elektron." },
  { "en": "Doping tipe-P artinya?", "id": "Kelebihan hole." },
  { "en": "Bahan paling umum dioda?", "id": "Silikon." },
  { "en": "Kurva karakteristik dioda?", "id": "IV Curve." },
  { "en": "Waktu pemulihan mundur dioda?", "id": "Trr." },
  { "en": "Dioda cepat diperlukan untuk?", "id": "Frekuensi tinggi." },
  { "en": "Dioda sebagai sensor suhu?", "id": "Termistor." },
  { "en": "Apa itu bridge rectifier?", "id": "Penyearah gelombang penuh." },
  { "en": "Dioda pada lampu LED?", "id": "LED itu sendiri." },
  { "en": "Dioda pada sistem tenaga surya?", "id": "Blocking dan bypass." },
  { "en": "Dioda sebagai pengubah level sinyal?", "id": "Level shifter." },
  { "en": "Dioda mencegah overcurrent?", "id": "Pembatas arus." },
  { "en": "Apa itu dynamic resistance?", "id": "Resistansi berubah." },
  { "en": "Dioda pada adaptor laptop?", "id": "Penyearah." },
  { "en": "Dioda untuk proteksi tegangan puncak?", "id": "TVS dioda." },
  { "en": "Dioda sinyal untuk apa?", "id": "Sinyal kecil." },
  { "en": "Dioda daya untuk apa?", "id": "Arus besar." },
  { "en": "Proses doping dioda?", "id": "Penambahan pengotor." },
  { "en": "Apa itu potential barrier?", "id": "Hambatan di junction." },
  { "en": "Potensial barrier germanium?", "id": "0.3 volt." },
  { "en": "Dioda ideal punya Ir?", "id": "Nol." },
  { "en": "Dioda nyata punya Ir?", "id": "Kecil." },
  { "en": "Peran dioda dalam sirkuit?", "id": "Kontrol aliran." },
  { "en": "Dioda untuk perlindungan ESD?", "id": "ESD protection diode." },
  { "en": "Dioda untuk sirkuit cepat?", "id": "Schottky." },
  { "en": "Dioda mengeluarkan panas saat?", "id": "Ada arus lewat." },
  { "en": "Dioda bisa digunakan sebagai?", "id": "Rectifier, switch, sensor." },
  { "en": "Dioda untuk modulasi frekuensi?", "id": "Varaktor." },
  { "en": "Dioda pada catu daya AC?", "id": "Penyearah." },
  { "en": "Dioda pada catu daya DC?", "id": "Tidak diperlukan." },
  { "en": "Apa itu dioda emisi?", "id": "LED." },
  { "en": "Apa itu dioda deteksi?", "id": "Fotodioda." },
  { "en": "Dioda dapat digambarkan sebagai?", "id": "Katup satu arah." },
  { "en": "Fungsi utama dioda dalam catu daya?", "id": "Menyearahkan." },
  { "en": "Bagaimana dioda menghasilkan cahaya?", "id": "Rekombinasi elektron-hole." },
  { "en": "Dioda pada rangkaian regulator?", "id": "Zener." },
  { "en": "Apa itu arus puncak non-repetitif?", "id": "Arus tunggal maksimum." },
  { "en": "Dioda untuk perlindungan tegangan rendah?", "id": "Tidak umum." },
  { "en": "Bagaimana dioda menghasilkan tegangan?", "id": "Efek fotovoltaik." },
  { "en": "Dioda dalam rangkaian multivibrator?", "id": "Stabilisasi frekuensi." },
  { "en": "Jenis dioda laser?", "id": "Quantum well." },
  { "en": "Apa itu 'elektron bebas'?", "id": "Pembawa muatan negatif." },
  { "en": "Dioda untuk pengujian isolasi?", "id": "Megohmmeter." },
  { "en": "Apa itu breakdown dioda Zener?", "id": "Tegangan balik stabil." },
  { "en": "Dioda dalam rangkaian penguat audio?", "id": "Penyearah." },
  { "en": "Apa itu doping tinggi?", "id": "Kepadatan pengotor tinggi." },
  { "en": "Dioda pada sistem komunikasi nirkabel?", "id": "Detektor, mixer." },
  { "en": "Apa itu dioda difusi?", "id": "Dioda junction planar." },
  { "en": "Dioda untuk deteksi radiasi alfa?", "id": "Detektor semikonduktor." },
  { "en": "Dioda dalam sirkuit memori?", "id": "Gerbang, dioda matriks." },
  { "en": "Apa itu resistansi dinamis dioda?", "id": "Resistansi kecil AC." },
  { "en": "Dioda untuk pengukuran level cairan?", "id": "Sensor." },
  { "en": "Apa itu kapasitansi transisi?", "id": "Kapasitansi bias mundur." },
  { "en": "Dioda untuk regulator tegangan?", "id": "Dioda Zener." },
  { "en": "Bagaimana dioda memengaruhi impedansi?", "id": "Bervariasi dengan bias." },
  { "en": "Dioda dalam rangkaian sampling?", "id": "Membuka/menutup cepat." },
  { "en": "Apa itu arus termoelektrik?", "id": "Arus dari perbedaan suhu." },
  { "en": "Dioda untuk detektor IR?", "id": "Fotodioda IR." },
  { "en": "Dioda sebagai sumber referensi arus?", "id": "Regulator arus." },
  { "en": "Apa itu efek breakdown?", "id": "Kerusakan permanen." },
  { "en": "Dioda untuk sirkuit proteksi arus lebih?", "id": "Fuse." },
  { "en": "Apa itu junction capacitance?", "id": "Kapasitansi PN junction." },
  { "en": "Dioda untuk sirkuit penggeser level?", "id": "Diode clamper." },
  { "en": "Dioda pada sirkuit detektor puncak?", "id": "Penyimpan puncak." },
  { "en": "Bagaimana dioda mempengaruhi sinyal DC?", "id": "Hanya dilewatkan." },
  { "en": "Apa itu reverse leakage current?", "id": "Arus bocor mundur." },
  { "en": "Dioda untuk sirkuit komparator?", "id": "Pembatas input." },
  { "en": "Dioda sebagai sensor gas?", "id": "Tidak umum." },
  { "en": "Apa itu forward dynamic resistance?", "id": "Hambatan maju AC." },
  { "en": "Dioda untuk penstabil arus?", "id": "Current-limiting diode." },
  { "en": "Dioda dalam sirkuit switching daya?", "id": "Rectifier cepat." },
  { "en": "Apa itu arus holding SCR?", "id": "Arus minimum penahan." },
  { "en": "Dioda untuk proteksi tegangan transien?", "id": "TVS dioda." },
  { "en": "Dioda untuk aplikasi tegangan rendah?", "id": "Germanium diode." },
  { "en": "Apa itu lapisan netral?", "id": "Bagian tengah PN junction." },
  { "en": "Dioda dalam sirkuit pengaman?", "id": "Pembatas tegangan." },
  { "en": "Bagaimana dioda membatasi arus?", "id": "Dengan Vf." },
  { "en": "Dioda pada pengisi daya baterai?", "id": "Pencegah overcharge." },
  { "en": "Apa itu dioda fotovoltaik?", "id": "Sel surya." },
  { "en": "Dioda untuk deteksi sinyal analog?", "id": "Diode detektor." },
  { "en": "Dioda dalam rangkaian osilator relaksasi?", "id": "Mengisi/mengosongkan." },
  { "en": "Apa itu tegangan knee dioda?", "id": "Tegangan aktif." },
  { "en": "Dioda untuk sirkuit gerbang AND?", "id": "Dioda AND gate." },
  { "en": "Dioda untuk aplikasi fotovoltaik?", "id": "Panel surya." },
  { "en": "Apa itu doping rendah?", "id": "Pengotor minimal." },
  { "en": "Apa perbedaan anoda dan katoda?", "id": "Polaritas." },
  { "en": "Dioda biasa digunakan sebagai apa?", "id": "Penyearah." },
  { "en": "Mengapa dioda tidak mengalirkan arus dua arah?", "id": "Struktur PN junction." },
  { "en": "Apa itu arus maju maksimum dioda?", "id": "Arus aman tertinggi." },
  { "en": "Dioda dalam rangkaian regulator tegangan?", "id": "Zener." },
  { "en": "Apa itu reverse current?", "id": "Arus bocor." },
  { "en": "Bagaimana dioda mengendalikan aliran arus?", "id": "Blokir satu arah." },
  { "en": "Dioda untuk mengubah cahaya menjadi listrik?", "id": "Fotodioda." },
  { "en": "Dioda untuk memancarkan cahaya?", "id": "LED." },
  { "en": "Apa itu tegangan knee?", "id": "Tegangan ambang konduksi." },
  { "en": "Dioda jenis apa yang sangat cepat?", "id": "Schottky." },
  { "en": "Apa fungsi dioda Zener?", "id": "Menstabilkan tegangan." },
  { "en": "Dioda dalam bridge rectifier?", "id": "Empat dioda." },
  { "en": "Bagaimana dioda menghasilkan panas?", "id": "Resistansi internal." },
  { "en": "Dioda untuk proteksi tegangan?", "id": "TVS." },
  { "en": "Apa itu diode drop?", "id": "Penurunan tegangan maju." },
  { "en": "Dioda pada panel surya?", "id": "Blocking dan bypass." },
  { "en": "Apa itu lapisan deplesi?", "id": "Zona kosong muatan." },
  { "en": "Bagaimana doping mempengaruhi konduktivitas?", "id": "Meningkatkan." },
  { "en": "Apa itu arus bocor termal?", "id": "Arus karena suhu." },
  { "en": "Dioda untuk membatasi amplitudo sinyal?", "id": "Clipper." },
  { "en": "Dioda untuk menggeser level DC?", "id": "Clamper." },
  { "en": "Apa itu waktu pemulihan balik?", "id": "Trr." },
  { "en": "Dioda cepat diperlukan untuk?", "id": "Frekuensi tinggi." },
  { "en": "Jenis doping yang menghasilkan hole?", "id": "P-tipe." },
  { "en": "Jenis doping yang menghasilkan elektron?", "id": "N-tipe." },
  { "en": "Material utama pembuatan dioda?", "id": "Silikon." },
  { "en": "Apa itu dioda tunnel?", "id": "Resistansi negatif." },
  { "en": "Dioda Gunn digunakan untuk?", "id": "Osilator gelombang mikro." },
  { "en": "Dioda laser menghasilkan apa?", "id": "Cahaya koheren." },
  { "en": "Dioda PIN digunakan di mana?", "id": "Aplikasi RF." },
  { "en": "Apa itu arus bias maju?", "id": "Arus lewat maju." },
  { "en": "Apa itu tegangan bias mundur?", "id": "Tegangan balik." },
  { "en": "Dioda untuk proteksi ESD?", "id": "TVS dioda." },
  { "en": "Apa itu barrier potential?", "id": "Tegangan penghalang." },
  { "en": "Dioda bisa digunakan sebagai sensor?", "id": "Ya." },
  { "en": "Breakdown Zener berfungsi sebagai?", "id": "Pengatur tegangan." },
  { "en": "TVS melindungi dari?", "id": "Lonjakan tegangan." },
  { "en": "Dioda mengubah AC jadi apa?", "id": "DC." },
  { "en": "Apa itu PIV rating?", "id": "Peak Inverse Voltage." },
  { "en": "PIV penting untuk?", "id": "Pemilihan dioda." },
  { "en": "Dioda pada catu daya?", "id": "Penyearah." },
  { "en": "Mengapa dioda vital?", "id": "Dasar elektronika daya." },
  { "en": "Apa itu Vf dioda?", "id": "Forward voltage drop." },
  { "en": "Vf dioda germanium?", "id": "0.3V." },
  { "en": "Dioda pada detektor gelombang?", "id": "Mendemodulasi sinyal." },
  { "en": "Dioda switching untuk apa?", "id": "Sirkuit cepat." },
  { "en": "Anoda adalah terminal apa?", "id": "Positif." },
  { "en": "Katoda adalah terminal apa?", "id": "Negatif." },
  { "en": "Dioda yang memancarkan cahaya?", "id": "LED." },
  { "en": "Dioda yang mendeteksi cahaya?", "id": "Fotodioda." },
  { "en": "Arus bocor dioda dipengaruhi oleh?", "id": "Suhu." },
  { "en": "Kapasitansi dioda dimanfaatkan di?", "id": "Varaktor." },
  { "en": "Dioda varaktor mengubah apa?", "id": "Kapasitansi." },
  { "en": "Aplikasi varaktor?", "id": "Tuning radio." },
  { "en": "Dioda dengan respons tercepat?", "id": "Schottky." },
  { "en": "Dioda untuk frekuensi mikro?", "id": "PIN diode." },
  { "en": "Dioda pada sirkuit detektor?", "id": "Mendemodulasi." },
  { "en": "Apa itu reverse breakdown voltage?", "id": "Tegangan balik rusak." },
  { "en": "Arus maju dioda?", "id": "Arus saat konduksi." },
  { "en": "Tegangan mundur dioda?", "id": "Tegangan saat blokir." },
  { "en": "Dioda untuk pemulihan cepat?", "id": "Fast recovery diode." },
  { "en": "Mengapa dioda menghasilkan panas?", "id": "Disipasi daya." },
  { "en": "Pendingin dioda berguna untuk?", "id": "Mencegah overheating." },
  { "en": "Dioda pada power supply linier?", "id": "Rectifier." },
  { "en": "Dioda ideal vs nyata?", "id": "Model vs perilaku aktual." },
  { "en": "Bagaimana dioda memblokir arus?", "id": "Hanya satu arah." },
  { "en": "Dioda untuk proteksi polaritas?", "id": "Mencegah salah sambung." },
  { "en": "Blocking diode untuk apa?", "id": "Mencegah arus balik." },
  { "en": "Dioda yang berkedip?", "id": "Blinking LED." },
  { "en": "Apa itu foto-dioda?", "id": "Sensor cahaya." },
  { "en": "Aplikasi foto-dioda?", "id": "Pendeteksi optik." },
  { "en": "Dioda untuk transmisi IR?", "id": "IR LED." },
  { "en": "Aplikasi IR LED?", "id": "Remote control." },
  { "en": "Dioda untuk sterilisasi?", "id": "UV LED." },
  { "en": "Dioda current regulator?", "id": "Pengatur arus konstan." },
  { "en": "Dioda pada power supply switching?", "id": "Penyearah keluaran." },
  { "en": "Dioda pada sirkuit clipper?", "id": "Memangkas amplitudo." },
  { "en": "Dioda untuk induktor?", "id": "Flyback diode." },
  { "en": "Dioda untuk menggandakan tegangan?", "id": "Voltage doubler." },
  { "en": "Metode pembuatan dioda?", "id": "Planar." },
  { "en": "Dioda memiliki daerah deplesi?", "id": "Zona tanpa muatan." },
  { "en": "Dioda sebagai sakelar?", "id": "On/off." },
  { "en": "Dioda sinyal untuk?", "id": "Aplikasi rendah daya." },
  { "en": "Dioda daya untuk?", "id": "Aplikasi tinggi daya." },
  { "en": "Suhu memengaruhi tegangan maju?", "id": "Vf menurun." },
  { "en": "Suhu memengaruhi arus bocor?", "id": "Ir meningkat." },
  { "en": "Dioda pada sirkuit detektor puncak?", "id": "Mendeteksi puncak." },
  { "en": "Dioda pada sirkuit limiter?", "id": "Membatasi level." },
  { "en": "Apa itu SCR dioda?", "id": "Sakelar daya." },
  { "en": "Apa itu TRIAC dioda?", "id": "Pengontrol AC." },
  { "en": "Dioda pada rangkaian timer?", "id": "Pengisian kapasitor." },
  { "en": "Dioda pada gerbang logika OR?", "id": "Digunakan dengan resistor." },
  { "en": "Dioda untuk perlindungan tegangan rendah?", "id": "Tidak umum." },
  { "en": "Bagaimana dioda menghasilkan tegangan?", "id": "Efek fotovoltaik." },
  { "en": "Dioda dalam rangkaian multivibrator?", "id": "Stabilisasi frekuensi." },
  { "en": "Jenis dioda laser?", "id": "Quantum well." },
  { "en": "Apa itu 'elektron bebas'?", "id": "Pembawa muatan negatif." },
  { "en": "Dioda untuk pengujian isolasi?", "id": "Megohmmeter." },
  { "en": "Apa itu breakdown dioda Zener?", "id": "Tegangan balik stabil." },
  { "en": "Dioda dalam rangkaian penguat audio?", "id": "Penyearah." },
  { "en": "Apa itu doping tinggi?", "id": "Kepadatan pengotor tinggi." },
  { "en": "Dioda pada sistem komunikasi nirkabel?", "id": "Detektor, mixer." },
  { "en": "Apa itu dioda difusi?", "id": "Dioda junction planar." },
  { "en": "Dioda untuk deteksi radiasi alfa?", "id": "Detektor semikonduktor." },
  { "en": "Dioda dalam sirkuit memori?", "id": "Gerbang, dioda matriks." },
  { "en": "Apa itu resistansi dinamis dioda?", "id": "Resistansi kecil AC." },
  { "en": "Dioda untuk pengukuran level cairan?", "id": "Sensor." },
  { "en": "Apa itu kapasitansi transisi?", "id": "Kapasitansi bias mundur." },
  { "en": "Dioda untuk regulator tegangan?", "id": "Dioda Zener." },
  { "en": "Bagaimana dioda memengaruhi impedansi?", "id": "Bervariasi dengan bias." },
  { "en": "Dioda dalam rangkaian sampling?", "id": "Membuka/menutup cepat." },
  { "en": "Apa itu arus termoelektrik?", "id": "Arus dari perbedaan suhu." },
  { "en": "Dioda untuk detektor IR?", "id": "Fotodioda IR." },
  { "en": "Dioda sebagai sumber referensi arus?", "id": "Regulator arus." },
  { "en": "Apa itu efek breakdown?", "id": "Kerusakan permanen." },
  { "en": "Dioda untuk sirkuit proteksi arus lebih?", "id": "Fuse." },
  { "en": "Apa itu junction capacitance?", "id": "Kapasitansi PN junction." },
  { "en": "Dioda untuk sirkuit penggeser level?", "id": "Diode clamper." },
  { "en": "Dioda pada sirkuit detektor puncak?", "id": "Penyimpan puncak." },
  { "en": "Bagaimana dioda mempengaruhi sinyal DC?", "id": "Hanya dilewatkan." },
  { "en": "Apa itu reverse leakage current?", "id": "Arus bocor mundur." },
  { "en": "Dioda untuk sirkuit komparator?", "id": "Pembatas input." },
  { "en": "Dioda sebagai sensor gas?", "id": "Tidak umum." },
  { "en": "Apa itu forward dynamic resistance?", "id": "Hambatan maju AC." },
  { "en": "Dioda untuk penstabil arus?", "id": "Current-limiting diode." },
  { "en": "Dioda dalam sirkuit switching daya?", "id": "Rectifier cepat." },
  { "en": "Apa itu arus holding SCR?", "id": "Arus minimum penahan." },
  { "en": "Dioda untuk proteksi tegangan transien?", "id": "TVS dioda." },
  { "en": "Dioda untuk aplikasi tegangan rendah?", "id": "Germanium diode." },
  { "en": "Apa itu lapisan netral?", "id": "Bagian tengah PN junction." },
  { "en": "Dioda dalam sirkuit pengaman?", "id": "Pembatas tegangan." },
  { "en": "Bagaimana dioda membatasi arus?", "id": "Dengan Vf." },
  { "en": "Dioda pada pengisi daya baterai?", "id": "Pencegah overcharge." },
  { "en": "Apa itu dioda fotovoltaik?", "id": "Sel surya." },
  { "en": "Dioda untuk deteksi sinyal analog?", "id": "Diode detektor." },
  { "en": "Dioda dalam rangkaian osilator relaksasi?", "id": "Mengisi/mengosongkan." },
  { "en": "Apa itu tegangan knee dioda?", "id": "Tegangan aktif." },
  { "en": "Dioda untuk sirkuit gerbang AND?", "id": "Dioda AND gate." },
  { "en": "Dioda untuk aplikasi fotovoltaik?", "id": "Panel surya." },
  { "en": "Apa itu doping rendah?", "id": "Pengotor minimal." },


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
            }, 6500);
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
