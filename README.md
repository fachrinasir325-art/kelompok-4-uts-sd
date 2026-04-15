#include <iostream>
using namespace std;

struct Buku {
    int id;
    string judul, penulis, genre;
    int tahun;
    bool tersedia;
};

class Perpustakaan {
private:
    Buku* data;
    int jumlah;
    int nextId;

    void urutkan() {
        for (int i = 0; i < jumlah - 1; i++) {
            for (int j = i + 1; j < jumlah; j++) {
                if (data[i].tahun > data[j].tahun) {
                    swap(data[i], data[j]);
                }
            }
        }
    }

public:
    Perpustakaan() {
        data = new Buku[100];
        jumlah = 0;
        nextId = 1;
    }

    void tambah() {
        if (jumlah >= 100) {
            cout << "Kapasitas penuh\n";
            return;
        }

        Buku b;
        b.id = nextId++;

        cin.ignore();
        cout << "Judul   : "; getline(cin, b.judul);
        cout << "Penulis : "; getline(cin, b.penulis);
        cout << "Genre   : "; getline(cin, b.genre);
        cout << "Tahun   : "; cin >> b.tahun;

        b.tersedia = true;
        data[jumlah++] = b;

        cout << "Buku ditambahkan\n";
    }

    void tampil() {
        if (jumlah == 0) {
            cout << "Tidak ada buku\n";
            return;
        }

        cout << "\nDaftar Buku:\n";
        for (int i = 0; i < jumlah; i++) {
            cout << data[i].id << ". " << data[i].judul
                 << " | " << data[i].penulis
                 << " | " << data[i].genre
                 << " | " << data[i].tahun
                 << " [" << (data[i].tersedia ? "Tersedia" : "Dipinjam") << "]\n";
        }
    }

    void pinjam() {
        int id;
        cout << "ID: "; cin >> id;

        for (int i = 0; i < jumlah; i++) {
            if (data[i].id == id) {
                if (data[i].tersedia) {
                    data[i].tersedia = false;
                    cout << "Dipinjam\n";
                } else {
                    cout << "Sudah dipinjam\n";
                }
                return;
            }
        }
        cout << "Tidak ditemukan\n";
    }

    void kembali() {
        int id;
        cout << "ID: "; cin >> id;

        for (int i = 0; i < jumlah; i++) {
            if (data[i].id == id) {
                if (!data[i].tersedia) {
                    data[i].tersedia = true;
                    cout << "Dikembalikan\n";
                } else {
                    cout << "Buku tidak dipinjam\n";
                }
                return;
            }
        }
        cout << "Tidak ditemukan\n";
    }

    void hapus() {
        int id;
        cout << "ID: "; cin >> id;

        for (int i = 0; i < jumlah; i++) {
            if (data[i].id == id) {
                for (int j = i; j < jumlah - 1; j++) {
                    data[j] = data[j + 1];
                }
                jumlah--;
                cout << "Dihapus\n";
                return;
            }
        }
        cout << "Tidak ditemukan\n";
    }

    void sortBuku() {
        if (jumlah == 0) {
            cout << "Tidak ada buku untuk diurutkan\n";
            return;
        }

        urutkan();
        cout << "Data berhasil diurutkan berdasarkan tahun\n";
    }
};

int main() {
    Perpustakaan p;
    int pilih;

    cout << "===================================================\n";
    cout << "= Selamat datang di sistem manajemen perpustakaan =\n";
    cout << "===================================================\n";

    do {
        cout << "\n=== MENU ===";
        cout << "\n1.Tambah buku\n2.Tampilkan buku\n3.Pinjam buku\n4.Kembali buku\n5.Hapus buku\n6.Sort\n0.Keluar\n";
        cout << "Pilih: ";
        cin >> pilih;

        switch (pilih) {
            case 1: p.tambah(); break;
            case 2: p.tampil(); break;
            case 3: p.pinjam(); break;
            case 4: p.kembali(); break;
            case 5: p.hapus(); break;
            case 6: p.sortBuku(); break;
            case 0:
                cout << "Keluar dari program...\n";
                break;
            default:
                cout << "Pilihan tidak valid\n";
        }

    } while (pilih != 0);

    return 0;
}
