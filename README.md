<!DOCTYPE html>
<html lang="id">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="theme-color" content="#087f5b">
    <title>Ulla Service</title>

    <style>
        * {
            box-sizing: border-box
        }

        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #f3f7f5;
            color: #18352b;
        }

        header {
            background: #087f5b;
            color: white;
            padding: 20px;
            border-radius: 0 0 24px 24px;
        }

        header h1 {
            margin: 0;
            font-size: 25px
        }

        header p {
            margin-bottom: 0;
            color: #e0fff1
        }

        .wrap {
            max-width: 750px;
            margin: auto;
            padding: 18px
        }

        .search {
            width: 100%;
            padding: 14px;
            border: 1px solid #d5e4dc;
            border-radius: 12px;
            font-size: 16px;
            margin: 10px 0 20px;
        }

        h2 {
            font-size: 20px
        }

        .services {
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            gap: 12px;
        }

        .card {
            background: white;
            padding: 15px;
            border-radius: 16px;
            box-shadow: 0 3px 12px #143d2510;
        }

        .emoji {
            font-size: 32px
        }

        .card h3 {
            font-size: 16px;
            margin: 8px 0
        }

        .price {
            font-weight: bold;
            color: #087f5b
        }

        button,
        .btn {
            border: 0;
            border-radius: 9px;
            padding: 11px;
            font-weight: bold;
            cursor: pointer;
            text-decoration: none;
            display: inline-block;
        }

        .order {
            background: #087f5b;
            color: white;
            width: 100%;
            margin-top: 12px
        }

        .primary {
            background: #087f5b;
            color: white
        }

        .panel {
            background: white;
            padding: 18px;
            border-radius: 16px;
            margin-top: 22px;
        }

        input,
        select,
        textarea {
            width: 100%;
            padding: 12px;
            margin: 7px 0 12px;
            border: 1px solid #ccdcd3;
            border-radius: 9px;
            font: inherit;
            background: white;
        }

        textarea {
            min-height: 85px
        }

        footer {
            text-align: center;
            padding: 25px;
            color: #64776d;
            font-size: 13px
        }

        .empty {
            grid-column: 1/-1;
            color: #64776d
        }

        @media(max-width:360px) {
            .services {
                grid-template-columns: 1fr
            }
        }
    </style>
</head>

<body>
    <header>
        <div class="wrap">
            <h1>🌿 Ulla Service</h1>
            <p>Temukan jasa yang kamu butuhkan dengan mudah.</p>
        </div>
    </header>

    <main class="wrap">
        <h2>Layanan tersedia</h2>
        <input id="search" class="search" placeholder="Cari jasa, misalnya desain poster..." aria-label="Cari layanan">

        <div id="services" class="services"></div>

        <section class="panel" id="form">
            <h2>Pesan layanan</h2>
            <p>Pilih layanan, isi informasi, lalu kirim permintaan melalui WhatsApp.</p>

            <form id="booking">
                <label for="nama">Nama kamu</label>
                <input id="nama" required maxlength="80" placeholder="Masukkan nama">

                <label for="pilihan">Layanan</label>
                <select id="pilihan" required></select>

                <label for="tanggal">Tanggal yang diinginkan</label>
                <input id="tanggal" type="date" required>

                <label for="catatan">Detail pesanan</label>
                <textarea id="catatan" maxlength="500" placeholder="Jelaskan kebutuhan kamu"></textarea>

                <button class="primary" type="submit">
                    Pesan melalui WhatsApp
                </button>
            </form>
        </section>

        <section class="panel">
            <h2>Untuk pemilik usaha</h2>
            <p>Ingin memiliki aplikasi katalog jasa seperti ini?</p>
            <p>Hubungi Ulla Digital untuk konsultasi pembuatan website atau aplikasi web.</p>
            <a class="btn primary" id="ownerLink" href="#" target="_blank" rel="noopener">
                Hubungi pembuat aplikasi
            </a>
        </section>
    </main>

    <footer>
        © <span id="year"></span> Ulla Service · Prototipe bisnis digital
    </footer>

    <script>
        "use strict";

        // GANTI dengan nomor WhatsApp milikmu.
        // Format: kode negara + nomor, tanpa tanda + atau spasi.
        const NOMOR_WA = "622299935062";

        const layanan = [
            {
                id: "desain",
                nama: "Desain Poster",
                harga: 35000,
                ikon: "🎨",
                deskripsi: "Poster promosi dan kegiatan."
            },
            {
                id: "website",
                nama: "Website Sederhana",
                harga: 300000,
                ikon: "💻",
                deskripsi: "Website profil usaha atau organisasi."
            },
            {
                id: "foto",
                nama: "Jasa Fotografi",
                harga: 100000,
                ikon: "📸",
                deskripsi: "Dokumentasi kegiatan dan produk."
            },
            {
                id: "konsultasi",
                nama: "Konsultasi Digital",
                harga: 25000,
                ikon: "📱",
                deskripsi: "Diskusi kebutuhan digital usaha."
            }
        ];

        const rupiah = n => new Intl.NumberFormat(
            "id-ID",
            {
                style: "currency", currency: "IDR",
                maximumFractionDigits: 0
            }
        ).format(n);

        const wadah = document.getElementById("services");
        const pilihan = document.getElementById("pilihan");

        function tampilkanLayanan(teks = "") {
            wadah.replaceChildren();

            const hasil = layanan.filter(x =>
                (x.nama + " " + x.deskripsi)
                    .toLowerCase().includes(teks.toLowerCase())
            );

            if (!hasil.length) {
                const p = document.createElement("p");
                p.className = "empty";
                p.textContent = "Layanan tidak ditemukan.";
                wadah.append(p);
                return;
            }

            hasil.forEach(x => {
                const kartu = document.createElement("article");
                kartu.className = "card";

                const ikon = document.createElement("div");
                ikon.className = "emoji";
                ikon.textContent = x.ikon;

                const judul = document.createElement("h3");
                judul.textContent = x.nama;

                const deskripsi = document.createElement("p");
                deskripsi.textContent = x.deskripsi;

                const harga = document.createElement("div");
                harga.className = "price";
                harga.textContent = rupiah(x.harga);

                const tombol = document.createElement("button");
                tombol.className = "order";
                tombol.textContent = "Pilih layanan";
                tombol.type = "button";
                tombol.addEventListener("click", () => {
                    pilihan.value = x.id;
                    document.getElementById("form")
                        .scrollIntoView({ behavior: "smooth" });
                });

                kartu.append(ikon, judul, deskripsi, harga, tombol);
                wadah.append(kartu);
            });
        }

        layanan.forEach(x => {
            const opt = document.createElement("option");
            opt.value = x.id;
            opt.textContent = x.nama + " — " + rupiah(x.harga);
            pilihan.append(opt);
        });

        document.getElementById("search")
            .addEventListener("input", e =>
                tampilkanLayanan(e.target.value));

        document.getElementById("booking")
            .addEventListener("submit", e => {
                e.preventDefault();

                if (!/^[1-9]\d{8,14}$/.test(NOMOR_WA)) {
                    alert("Pemilik perlu mengganti nomor WhatsApp contoh di kode.");
                    return;
                }

                const nama = document.getElementById("nama").value.trim();
                const tanggal = document.getElementById("tanggal").value;
                const catatan = document.getElementById("catatan").value.trim();
                const item = layanan.find(x => x.id === pilihan.value);

                if (!nama || !tanggal || !item) {
                    alert("Mohon lengkapi informasi pesanan.");
                    return;
                }

                const pesan = [
                    "Halo, saya ingin memesan layanan.",
                    "Nama: " + nama,
                    "Layanan: " + item.nama,
                    "Harga indikatif: " + rupiah(item.harga),
                    "Tanggal yang diinginkan: " + tanggal,
                    "Detail: " + (catatan || "-")
                ].join("\n");

                const url = "https://wa.me/" + NOMOR_WA +
                    "?text=" + encodeURIComponent(pesan);

                window.open(url, "_blank", "noopener");
            });

        document.getElementById("ownerLink").href =
            "https://wa.me/" + NOMOR_WA +
            "?text=" + encodeURIComponent(
                "Halo, saya ingin konsultasi pembuatan aplikasi jasa."
            );

        document.getElementById("year").textContent =
            new Date().getFullYear();

        tampilkanLayanan();
    </script>
</body>

</html>
