# Penjana Borang OPR Warden SMSTJ

Laman ini menjana OPR PDF dengan pratonton langsung, mengarkib rekod dalam Google Sheets, menyimpan PDF dalam Google Drive mengikut bulan, dan menyediakan muat turun mingguan serta ZIP. Tarikh menggunakan format dd/mm/yyyy. Tiada sambungan Supabase diperlukan oleh versi ini.

## 1. Sediakan Google Sheet

1. Cipta satu Google Sheet dan satu tab bernama `OPR Records`.
2. Pilihan: biarkan baris pertama kosong; Apps Script akan mengisi header. Jika anda masukkan header sendiri, sel A1 hingga I1 mesti tepat seperti ini:

   `Created At | Warden | Date | Time | Place | Notes | Drive File ID | Drive Link | Drive Folder ID`

3. Salin Sheet ID daripada URL. Ia ialah bahagian di antara `/d/` dan `/edit`.

## 2. Sediakan Google Apps Script

1. Dalam Apps Script, gantikan semua kandungan `Code.gs` dengan fail `apps-script/Code.gs` daripada pakej ini.
2. Di **Project Settings > Script Properties**, sediakan:
   - `ROOT_FOLDER_ID` = ID folder Drive sahaja
   - `SHARED_TOKEN` = token rawak panjang yang sama dengan Netlify
   - `SPREADSHEET_ID` = ID Google Sheet sahaja
   - `SHEET_NAME` = `OPR Records`
3. Simpan dan pilih **Deploy > Manage deployments**. Edit deployment web app sedia ada, pilih **New version**, dan deploy.
4. Tetapkan **Execute as** kepada akaun anda dan akses web app kepada sesiapa yang perlu menggunakannya. Benarkan akses Drive dan Sheets apabila diminta.
5. Gunakan URL web app deployment yang berakhir dengan `/exec`.

## 3. Tetapkan Netlify

Di **Project configuration > Environment variables**, tetapkan skop **Functions**:

- `APPS_SCRIPT_URL` = URL web app Apps Script `/exec`
- `APPS_SCRIPT_SHARED_TOKEN` = nilai sama dengan `SHARED_TOKEN` dalam Script Properties

`SUPABASE_URL` dan `SUPABASE_SECRET_KEY` tidak digunakan oleh versi ini. Jangan letakkan token dalam fail frontend atau GitHub.

## 4. Deploy melalui GitHub dan Netlify

Muat naik kandungan pakej ini ke root repositori GitHub, kekalkan folder `assets`, `apps-script` dan `netlify/functions`, kemudian commit. Netlify akan deploy secara automatik jika repositori sudah disambungkan. Fail `netlify.toml` menetapkan publish directory dan folder functions; tiada build command diperlukan untuk laman statik ini.

Selepas perubahan Environment Variables disimpan, trigger satu deploy baharu di Netlify.

## 5. Data OPR terdahulu

Rekod baharu akan disimpan dalam Google Sheets dan PDF dalam Google Drive. Rekod yang sedia ada dalam Supabase tidak dipindahkan secara automatik; arkib versi ini hanya membaca tab `OPR Records`.

## Semakan

Hantar satu OPR dengan empat gambar. Pastikan PDF disimpan dalam folder bulan yang betul, satu baris ditambah ke tab `OPR Records`, dan PDF boleh dimuat turun daripada arkib. PDF maksimum 4.4 MB.
