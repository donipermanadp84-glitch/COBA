<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TTS Sejarah XI: Pendudukan Jepang (1942-1945)</title>
    <style>
        :root {
            --primary: #8b0000;
            --secondary: #d4af37;
            --bg: #f4f1ea;
            --card: #ffffff;
            --text: #2c2c2c;
            --correct: #2e7d32;
            --wrong: #c62828;
            --clue-cell: #fdf3d0;
        }
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        header {
            text-align: center;
            background: var(--primary);
            color: white;
            padding: 15px 30px;
            border-radius: 10px;
            border-bottom: 5px solid var(--secondary);
            width: 100%;
            max-width: 1200px;
            margin-bottom: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        }
        header h1 { margin: 0; font-size: 1.5rem; letter-spacing: 1px; }
        header p { margin: 5px 0 0; font-size: 0.95rem; opacity: 0.9; }
        
        .main-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: center;
            max-width: 1200px;
            width: 100%;
        }

        /* Grid TTS */
        .board-section {
            background: var(--card);
            padding: 18px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        #grid {
            display: grid;
            grid-template-columns: repeat(18, 30px);
            grid-template-rows: repeat(18, 30px);
            gap: 2px;
            background-color: #222;
            padding: 4px;
            border-radius: 5px;
        }
        .cell {
            position: relative;
            width: 30px;
            height: 30px;
            background-color: #1c1c1c;
        }
        .cell.active {
            background-color: #fff;
        }
        .cell.prefilled {
            background-color: var(--clue-cell);
        }
        .cell input {
            width: 100%;
            height: 100%;
            border: none;
            text-align: center;
            font-size: 15px;
            font-weight: bold;
            text-transform: uppercase;
            background: transparent;
            color: #111;
            outline: none;
            padding: 0;
        }
        .cell.prefilled input {
            color: #7a5901;
            cursor: default;
        }
        .cell input:focus:not([readonly]) {
            background-color: #e3f2fd;
        }
        .cell.correct input {
            background-color: #c8e6c9;
            color: var(--correct);
        }
        .cell.wrong input {
            background-color: #ffcdd2;
            color: var(--wrong);
        }
        .cell-number {
            position: absolute;
            top: 1px;
            left: 2px;
            font-size: 8.5px;
            font-weight: bold;
            color: var(--primary);
            pointer-events: none;
        }

        .legend {
            margin-top: 10px;
            font-size: 0.85rem;
            color: #555;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .legend-box {
            display: inline-block;
            width: 16px;
            height: 16px;
            background-color: var(--clue-cell);
            border: 1px solid #bca048;
            vertical-align: middle;
        }

        /* Controls */
        .controls {
            margin-top: 12px;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
        }
        button {
            padding: 9px 16px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
            font-size: 0.88rem;
        }
        .btn-check { background: var(--correct); color: white; }
        .btn-check:hover { background: #1b5e20; }
        .btn-hint { background: var(--secondary); color: #111; }
        .btn-hint:hover { background: #bfa030; }
        .btn-reset { background: #666; color: white; }
        .btn-reset:hover { background: #444; }

        #score-board {
            margin-top: 10px;
            font-weight: bold;
            font-size: 1.05rem;
            color: var(--primary);
        }

        /* Clues Section */
        .clues-section {
            flex: 1;
            min-width: 340px;
            background: var(--card);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            max-height: 660px;
            overflow-y: auto;
        }
        .clues-section h2 {
            margin-top: 0;
            color: var(--primary);
            border-bottom: 2px solid var(--secondary);
            padding-bottom: 5px;
            font-size: 1.15rem;
        }
        .clue-item {
            margin-bottom: 8px;
            font-size: 0.9rem;
            line-height: 1.4;
            padding: 6px 8px;
            border-radius: 5px;
            cursor: pointer;
            border-left: 3px solid transparent;
        }
        .clue-item:hover {
            background-color: #f9f5eb;
            border-left: 3px solid var(--primary);
        }

        @media (max-width: 680px) {
            #grid {
                grid-template-columns: repeat(18, 19px);
                grid-template-rows: repeat(18, 19px);
            }
            .cell { width: 19px; height: 19px; }
            .cell input { font-size: 11px; }
            .cell-number { font-size: 6.5px; }
        }
    </style>
</head>
<body>

<header>
    <h1>TEKA-TEKI SILANG SEJARAH INDONESIA (KELAS XI)</h1>
    <p>Materi: Pendudukan Militer Jepang di Indonesia (1942–1945) | Kurikulum Merdeka</p>
</header>

<div class="main-container">
    <div class="board-section">
        <div id="grid"></div>
        <div class="legend">
            <span class="legend-box"></span> Kotak krem adalah huruf kunci bantuan awal
        </div>
        <div class="controls">
            <button class="btn-check" onclick="checkAnswers()">Periksa Jawaban</button>
            <button class="btn-hint" onclick="giveHint()">Bantuan (1 Huruf)</button>
            <button class="btn-reset" onclick="resetGame()">Reset TTS</button>
        </div>
        <div id="score-board">Skor: 0 / 100</div>
    </div>

    <div class="clues-section">
        <h2>MENDATAR (10 Soal)</h2>
        <div id="across-clues"></div>

        <h2 style="margin-top: 20px;">MENURUN (10 Soal)</h2>
        <div id="down-clues"></div>
    </div>
</div>

<script>
    const gridSize = 18;
    
    // 20 Soal Terhubung (10 Mendatar, 10 Menurun)
    const words = [
        // MENDATAR
        { num: 1,  dir: 'across', row: 1,  col: 1,  word: 'ROMUSHA',     clue: 'Sistem kerja paksa pada masa pendudukan Jepang yang sangat menyengsarakan rakyat.' },
        { num: 4,  dir: 'across', row: 3,  col: 0,  word: 'SEIKEREI',    clue: 'Upacara penghormatan dengan membungkukkan badan ke arah matahari terbit (Kaisar Jepang).' },
        { num: 6,  dir: 'across', row: 5,  col: 3,  word: 'HEIHO',       clue: 'Pasukan pembantu prajurit Jepang yang langsung ditempatkan dalam struktur militer Jepang.' },
        { num: 8,  dir: 'across', row: 7,  col: 6,  word: 'TONARIGUMI',  clue: 'Sistem rukun tetangga (RT) bentukan Jepang untuk mengawasi gerak-gerik warga hingga tingkat bawah.' },
        { num: 10, dir: 'across', row: 9,  col: 1,  word: 'SEINENDAN',   clue: 'Organisasi barisan pemuda bentukan Jepang untuk mendidik pemuda berusia 14–22 tahun.' },
        { num: 12, dir: 'across', row: 11, col: 4,  word: 'CHUOSANGIIN', clue: 'Dewan Pertimbangan Pusat yang dibentuk Jepang untuk memberi saran kepada Saiko Shikikan.' },
        { num: 14, dir: 'across', row: 13, col: 0,  word: 'PUTERA',      clue: 'Organisasi propaganda Jepang yang dipimpin oleh Empat Serangkai.' },
        { num: 16, dir: 'across', row: 14, col: 8,  word: 'JUGUNIANFU',  clue: 'Istilah bagi perempuan Indonesia dan Asia yang dipaksa menjadi wanita penghibur tentara Jepang.' },
        { num: 18, dir: 'across', row: 16, col: 1,  word: 'BPUPKI',      clue: 'Badan bentukan Jepang (Dokuritsu Junbi Cosakai) yang bertugas menyelidiki persiapan kemerdekaan.' },
        { num: 20, dir: 'across', row: 17, col: 10, word: 'KALIJATI',    clue: 'Tempat ditandatanganinya perjanjian penyerahan tanpa syarat Belanda kepada Jepang pada 8 Maret 1942.' },

        // MENURUN
        { num: 2,  dir: 'down',   row: 0,  col: 4,  word: 'OSAMUSEIREI', clue: 'Undang-undang yang dikeluarkan Panglima Tentara ke-16 Jepang sebagai dasar pemerintahan militer di Jawa.' },
        { num: 3,  dir: 'down',   row: 1,  col: 7,  word: 'AUTARKI',     clue: 'Sistem ekonomi perang Jepang di mana setiap daerah wajib memenuhi kebutuhannya sendiri dan menopang perang.' },
        { num: 5,  dir: 'down',   row: 2,  col: 11, word: 'KEIBODAN',    clue: 'Barisan pembantu polisi yang beranggotakan pemuda berusia 23–35 tahun.' },
        { num: 7,  dir: 'down',   row: 4,  col: 14, word: 'GUNSEIKAN',   clue: 'Jabatan kepala pemerintahan militer Jepang di Indonesia yang dijabat oleh kepala staf tentara.' },
        { num: 9,  dir: 'down',   row: 6,  col: 2,  word: 'PETA',        clue: 'Organisasi militer bentukan Jepang yang dipimpin perwira pribumi (seperti Supriyadi) dan menjadi cikal bakal TNI.' },
        { num: 11, dir: 'down',   row: 8,  col: 9,  word: 'FUJINKAI',    clue: 'Himpunan wanita bentukan Jepang untuk memobilisasi kaum perempuan mendukung garis belakang perang.' },
        { num: 13, dir: 'down',   row: 10, col: 16, word: 'JAWABARU',    clue: 'Surat kabar resmi yang diterbitkan pada masa pendudukan Jepang sebagai media propaganda.' },
        { num: 15, dir: 'down',   row: 12, col: 6,  word: 'KIMIGAYO',    clue: 'Lagu kebangsaan Jepang yang wajib dinyanyikan siswa di sekolah-sekolah pada masa pendudukan.' },
        { num: 17, dir: 'down',   row: 13, col: 12, word: 'NIPPON',      clue: 'Sebutan lain untuk negara Jepang yang sering digunakan dalam semboyan propaganda Gerakan Tiga A.' },
        { num: 19, dir: 'down',   row: 11, col: 1,  word: 'SUISHINTAI',  clue: 'Nama lain dalam bahasa Jepang untuk Barisan Pelopor yang dipimpin oleh Ir. Soekarno.' }
    ];

    // Daftar koordinat huruf kunci bantuan awal (format: "baris-kolom")
    const clueKeys = new Set([
        "1-1",   // R pada ROMUSHA
        "1-4",   // U (persilangan ROMUSHA & OSAMUSEIREI)
        "3-2",   // I pada SEIKEREI
        "3-7",   // I (persilangan SEIKEREI & AUTARKI)
        "5-4",   // E (persilangan HEIHO & OSAMUSEIREI)
        "7-7",   // O (persilangan TONARIGUMI & AUTARKI)
        "7-11",  // A (persilangan TONARIGUMI & KEIBODAN)
        "7-14",  // I (persilangan TONARIGUMI & GUNSEIKAN)
        "9-2",   // E (persilangan SEINENDAN & PETA)
        "9-4",   // N (persilangan SEINENDAN & OSAMUSEIREI)
        "11-6",  // O pada CHUOSANGIIN
        "11-9",  // N (persilangan CHUOSANGIIN & FUJINKAI)
        "13-1",  // U (persilangan PUTERA & SUISHINTAI)
        "14-9",  // U (persilangan JUGUNIANFU & FUJINKAI)
        "14-12", // I (persilangan JUGUNIANFU & NIPPON)
        "14-16", // A (persilangan JUGUNIANFU & JAWABARU)
        "16-1",  // B (persilangan BPUPKI & SUISHINTAI)
        "16-6",  // I (persilangan BPUPKI & KIMIGAYO)
        "17-12", // L (persilangan KALIJATI & NIPPON)
        "17-16"  // T (persilangan KALIJATI & JAWABARU)
    ]);

    const gridEl = document.getElementById('grid');
    const acrossEl = document.getElementById('across-clues');
    const downEl = document.getElementById('down-clues');
    let cellMap = {};
    let lastFocusedInput = null;

    function initGame() {
        gridEl.innerHTML = '';
        acrossEl.innerHTML = '';
        downEl.innerHTML = '';
        cellMap = {};
        lastFocusedInput = null;

        // Buat sel kosong 18x18
        for (let r = 0; r < gridSize; r++) {
            for (let c = 0; c < gridSize; c++) {
                const cell = document.createElement('div');
                cell.className = 'cell';
                cell.id = `cell-${r}-${c}`;
                gridEl.appendChild(cell);
            }
        }

        // Isi sel aktif berdasarkan kata
        words.forEach(item => {
            // Tampilkan daftar soal (tanpa label mudah/sedang/sulit)
            const clueDiv = document.createElement('div');
            clueDiv.className = 'clue-item';
            clueDiv.innerHTML = `<strong>${item.num}.</strong> ${item.clue} <em>(${item.word.length} huruf)</em>`;
            clueDiv.onclick = () => focusWord(item);
            
            if (item.dir === 'across') acrossEl.appendChild(clueDiv);
            else downEl.appendChild(clueDiv);

            for (let i = 0; i < item.word.length; i++) {
                const r = item.dir === 'across' ? item.row : item.row + i;
                const c = item.dir === 'across' ? item.col + i : item.col;
                const key = `${r}-${c}`;
                const cell = document.getElementById(`cell-${key}`);

                if (!cellMap[key]) {
                    cell.classList.add('active');
                    const input = document.createElement('input');
                    input.maxLength = 1;
                    input.dataset.answer = item.word[i];
                    input.dataset.row = r;
                    input.dataset.col = c;

                    const isPrefilled = clueKeys.has(key);
                    if (isPrefilled) {
                        cell.classList.add('prefilled');
                        input.value = item.word[i];
                        input.readOnly = true;
                    } else {
                        input.addEventListener('focus', () => { lastFocusedInput = input; });
                        input.addEventListener('input', (e) => handleInput(e, r, c));
                    }
                    
                    input.addEventListener('keydown', (e) => handleKeyDown(e, r, c));

                    cell.appendChild(input);
                    cellMap[key] = { input, dirs: [item.dir], isPrefilled };
                } else {
                    cellMap[key].dirs.push(item.dir);
                }

                if (i === 0) {
                    const numSpan = document.createElement('span');
                    numSpan.className = 'cell-number';
                    numSpan.textContent = item.num;
                    cell.appendChild(numSpan);
                }
            }
        });
    }

    function focusWord(item) {
        // Cari kotak pertama pada kata tersebut yang belum terisi (bukan huruf kunci)
        for (let i = 0; i < item.word.length; i++) {
            const r = item.dir === 'across' ? item.row : item.row + i;
            const c = item.dir === 'across' ? item.col + i : item.col;
            const key = `${r}-${c}`;
            if (cellMap[key] && !cellMap[key].isPrefilled) {
                cellMap[key].input.dataset.currentDir = item.dir;
                cellMap[key].input.focus();
                return;
            }
        }
    }

    function handleInput(e, r, c) {
        const input = e.target;
        input.parentElement.classList.remove('correct', 'wrong');
        if (input.value.length === 1) {
            const dir = input.dataset.currentDir || cellMap[`${r}-${c}`].dirs[0];
            moveNext(r, c, dir, 1);
        }
    }

    function moveNext(r, c, dir, step) {
        let currR = r;
        let currC = c;
        while (true) {
            currR = dir === 'down' ? currR + step : currR;
            currC = dir === 'across' ? currC + step : currC;
            const nextCell = cellMap[`${currR}-${currC}`];
            if (!nextCell) break;
            // Lompati kotak huruf kunci yang readonly agar langsung menuju kotak kosong berikutnya
            if (!nextCell.isPrefilled) {
                nextCell.input.dataset.currentDir = dir;
                nextCell.input.focus();
                break;
            }
        }
    }

    function handleKeyDown(e, r, c) {
        let nextR = r, nextC = c;
        if (e.key === 'ArrowRight') nextC++;
        else if (e.key === 'ArrowLeft') nextC--;
        else if (e.key === 'ArrowDown') nextR++;
        else if (e.key === 'ArrowUp') nextR--;
        else if (e.key === 'Backspace' && e.target.value === '') {
            const dir = e.target.dataset.currentDir || cellMap[`${r}-${c}`].dirs[0];
            e.preventDefault();
            moveNext(r, c, dir, -1);
            return;
        } else {
            return;
        }

        if (cellMap[`${nextR}-${nextC}`]) {
            e.preventDefault();
            cellMap[`${nextR}-${nextC}`].input.focus();
        }
    }

    function checkAnswers() {
        let total = 0;
        let correct = 0;

        Object.values(cellMap).forEach(({ input, isPrefilled }) => {
            if (isPrefilled) return; // Huruf kunci tidak dihitung dalam skor isian siswa
            total++;
            const userVal = input.value.toUpperCase().trim();
            const ansVal = input.dataset.answer;
            input.parentElement.classList.remove('correct', 'wrong');

            if (userVal === ansVal) {
                correct++;
                input.parentElement.classList.add('correct');
            } else if (userVal !== '') {
                input.parentElement.classList.add('wrong');
            }
        });

        const score = Math.round((correct / total) * 100);
        const board = document.getElementById('score-board');
        board.textContent = `Skor: ${score} / 100 (${correct} dari ${total} kotak isian benar)`;
        
        if (score === 100) {
            alert("Luar Biasa! Semua 20 jawaban benar! Anda menguasai materi Pendudukan Jepang di Indonesia.");
        }
    }

    function giveHint() {
        if (!lastFocusedInput || lastFocusedInput.readOnly) {
            alert("Klik salah satu kotak putih yang masih kosong terlebih dahulu untuk menggunakan bantuan!");
            return;
        }
        lastFocusedInput.value = lastFocusedInput.dataset.answer;
        lastFocusedInput.parentElement.classList.remove('wrong');
        lastFocusedInput.parentElement.classList.add('correct');
    }

    function resetGame() {
        if (confirm("Ulangi permainan dari awal? (Huruf kunci bantuan tetap dipertahankan)")) {
            initGame();
            document.getElementById('score-board').textContent = "Skor: 0 / 100";
        }
    }

    initGame();
</script>

</body>
</html>
