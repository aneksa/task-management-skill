---
name: multica-task-manager
description: Mengelola task untuk project Multica — membuat task baru, memperbarui status, menandai Definition of Done, dan menampilkan daftar task, dengan format terstruktur baku (Deskripsi, Scope, Referensi, Steps, Acceptance, DoD termasuk branch git). Gunakan skill ini setiap kali user minta "buatkan task", "task baru untuk multica", "update status task", "tandai task selesai/done", "list task", atau menyebut task Multica dalam bentuk apa pun — bahkan jika user hanya memberi judul/topik task tanpa detail lengkap.
---

# Multica Task Manager

Skill untuk membuat dan mengelola task project Multica sebagai file Markdown terstruktur,
sehingga setiap task konsisten formatnya dan siap dipakai sebagai acuan kerja (termasuk
alur branch git).

## Lokasi Penyimpanan

Semua task disimpan sebagai file Markdown di direktori `tasks/` pada root repo/project
yang sedang dikerjakan (buat direktori ini jika belum ada). Satu file = satu task.

Nama file: `{uid}-{task-slug}.md`
- `uid` — 6 karakter hex acak, lowercase (generate dengan `openssl rand -hex 3` atau
  `python3 -c "import uuid;print(uuid.uuid4().hex[:6])"`).
- `task-slug` — judul task dalam kebab-case, singkat (mis. `tambah-role-approver`).

Contoh nama file: `a3f9c1-tambah-role-approver.md`

## Format Task (Wajib)

Setiap file task memakai frontmatter YAML + body dengan section berikut, dalam urutan ini:

```markdown
---
task_name: tambah-role-approver
uid: a3f9c1
status: todo            # todo | in_progress | in_review | done
branch: features/tambah-role-approver-a3f9c1
created_at: 2026-08-19
---

# <Judul Task yang jelas dan actionable>

## Deskripsi
<Konteks & tujuan task — kenapa task ini perlu dikerjakan>

## Scope
<Batasan eksplisit: apa yang termasuk dan (jika perlu) apa yang TIDAK termasuk>

## Referensi
<File/class/module yang relevan, mis. `app\Models\User`, `app/Http/Controllers/UserController.php`>

## Steps
1. <Langkah 1, sertakan command konkret jika ada, mis. `php artisan make:model NewModel`>
2. <Langkah 2, mis. `php artisan make:controller ModelController`>
3. ...

## Acceptance
- [ ] <Kriteria yang bisa diverifikasi, mis. "Endpoint /api/users mengembalikan role approver">
- [ ] ...

## DoD (Definition of Done)
- [ ] Branch baru dibuat: `features/{task_name}-{uid}`
- [ ] Semua Acceptance criteria terpenuhi
- [ ] Kode di-review (jika berlaku)
- [ ] Merge ke default branch
```

Aturan isi:
- **Steps** harus konkret dan actionable — command CLI ditulis persis (mis.
  `php artisan make:model NewModel`), bukan deskripsi abstrak seperti "buat model".
- **Acceptance** ditulis sebagai checklist (`- [ ]`), tiap item harus bisa diverifikasi
  ya/tidak (bukan kalimat opini).
- **DoD** selalu memuat minimal dua item baku: buat branch `features/{task_name}-{uid}`
  dan merge ke default branch — tambahkan item lain jika user/project minta (mis. test
  lulus, review approved), tapi jangan hapus dua item baku ini.
- `branch` di frontmatter harus persis sama dengan `task_name` + `uid`, format
  `features/{task_name}-{uid}`.

## Cara Kerja

### Membuat task baru
1. Kumpulkan informasi dari user: judul/topik, deskripsi, scope, referensi file/class,
   langkah kerja. Jika user hanya kasih topik singkat, susun Deskripsi/Scope/Steps/
   Acceptance yang masuk akal berdasarkan konteks project, lalu nyatakan asumsi tersebut
   secara singkat ke user (bukan menolak membuat task karena info kurang lengkap).
2. Buat `task_name` (slug) dari judul task dan generate `uid` (6 hex acak).
3. Isi template di atas lengkap, termasuk `branch: features/{task_name}-{uid}`.
4. Simpan ke `tasks/{uid}-{task_name}.md`.
5. Konfirmasi ke user: nama file, branch yang akan dipakai, dan ringkasan Acceptance/DoD.

### Update status / menandai progres
- Ubah field `status` di frontmatter sesuai tahap (`todo` → `in_progress` → `in_review` →
  `done`).
- Saat item Acceptance atau DoD selesai, ubah `- [ ]` jadi `- [x]` pada item terkait —
  jangan centang item yang belum benar-benar terverifikasi.
- Set `status: done` hanya jika seluruh item DoD sudah tercentang.

### Menampilkan daftar task
- List isi direktori `tasks/`, baca frontmatter tiap file, tampilkan ringkas: `uid`,
  judul, `status`, `branch`. Kelompokkan berdasarkan `status` jika task lebih dari
  beberapa buah.

### Menghapus / membatalkan task
- Hanya hapus file task bila user secara eksplisit meminta ("batalkan task X", "hapus
  task X").

## Template

`templates/task-template.md` — kerangka kosong sesuai format di atas, siap diisi.
