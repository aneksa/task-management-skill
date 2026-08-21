---
task_name: tambah-role-approver
uid: a3f9c1
status: in_progress
branch: features/tambah-role-approver-a3f9c1
created_at: 2026-08-19
---

# Tambah Role Approver pada Modul User

## Deskripsi
Saat ini sistem hanya punya role `admin` dan `staff`. Tim finance butuh role baru
`approver` yang bisa menyetujui pengajuan tanpa akses penuh admin. Task ini menambahkan
role tersebut di level model, migration, dan authorization.

## Scope
- Tambah role `approver` di enum/kolom role User.
- Tambah policy/gate untuk aksi `approve` yang hanya bisa diakses role `approver` dan `admin`.
- Tidak termasuk: perubahan UI frontend (ditangani task terpisah).

## Referensi
- `app/Models/User.php`
- `app/Policies/ApprovalPolicy.php`
- `database/migrations/2024_01_10_000000_create_users_table.php`

## Steps
1. Buat migration baru: `php artisan make:migration add_role_approver_to_users_table`
2. Tambahkan value `approver` ke enum kolom `role` di migration tersebut.
3. Buat policy baru: `php artisan make:policy ApprovalPolicy`
4. Implementasikan method `approve()` di `ApprovalPolicy` untuk mengizinkan role `approver` dan `admin`.
5. Update seeder `UserSeeder` agar ada minimal satu user dengan role `approver` untuk testing lokal.

## Acceptance
- [x] Migration berhasil dijalankan tanpa error (`php artisan migrate`)
- [ ] User dengan role `approver` bisa mengakses endpoint approve
- [ ] User dengan role `staff` mendapat 403 saat mengakses endpoint approve

## DoD (Definition of Done)
- [x] Branch baru dibuat: `features/tambah-role-approver-a3f9c1`
- [ ] Semua Acceptance criteria terpenuhi
- [ ] Buat PR ke branch develop
