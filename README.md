#include <iostream>
#include <string>
using namespace std;

struct Pasien {
    int id, usia, prioritas;
    string nama, poli, status, noAntrian;
};

struct Riwayat {
    Pasien p;
};

class Sistem {
private:
    Pasien antrian[100];
    Riwayat riwayat[100];
    int jumlah = 0, selesai = 0;

    int id = 1, d=0, p=0, n=0;

    string poliList[4] = {"","Umum","Anak","Gigi"};

    string noAnt(int pr) {
        if(pr==1) return "D"+to_string(++d);
        if(pr==2) return "P"+to_string(++p);
        return "N"+to_string(++n);
    }

    void sortPrioritas() {
        for(int i=0;i<jumlah-1;i++) {
            for(int j=0;j<jumlah-i-1;j++) {
                if(antrian[j].prioritas > antrian[j+1].prioritas) {
                    swap(antrian[j], antrian[j+1]);
                }
            }
        }
    }

public:
    void daftar() {
        Pasien x;
        x.id = id++;
        x.status = "Menunggu";

        cout<<"Nama: "; cin>>x.nama;
        cout<<"Usia: "; cin>>x.usia;

        int p;
        cout<<"Poli (1.Umum 2.Anak 3.Gigi): ";
        cin>>p;
        x.poli = poliList[p];

        cout<<"Prioritas (1.Darurat 2.Prioritas 3.Normal): ";
        cin>>x.prioritas;

        x.noAntrian = noAnt(x.prioritas);

        antrian[jumlah++] = x;
        sortPrioritas();

        cout<<"No Antrian: "<<x.noAntrian<<"\n";
    }

    void tampil() {
        for(int i=0;i<jumlah;i++) {
            cout<<i+1<<". "<<antrian[i].nama
                <<" | "<<antrian[i].noAntrian
                <<" | "<<antrian[i].status<<"\n";
        }
    }

    void panggil() {
        for(int i=0;i<jumlah;i++) {
            if(antrian[i].status=="Menunggu") {
                antrian[i].status="Dipanggil";
                cout<<"Panggil: "<<antrian[i].nama<<"\n";
                return;
            }
        }
        cout<<"Tidak ada\n";
    }

    void selesaiPasien() {
        string no;
        cout<<"No Antrian: "; cin>>no;

        for(int i=0;i<jumlah;i++) {
            if(antrian[i].noAntrian==no) {
                riwayat[selesai++].p = antrian[i];

                for(int j=i;j<jumlah-1;j++)
                    antrian[j]=antrian[j+1];

                jumlah--;
                cout<<"Selesai\n";
                return;
            }
        }
        cout<<"Tidak ditemukan\n";
    }

    void batal() {
        string no;
        cout<<"No Antrian: "; cin>>no;

        for(int i=0;i<jumlah;i++) {
            if(antrian[i].noAntrian==no) {
                for(int j=i;j<jumlah-1;j++)
                    antrian[j]=antrian[j+1];

                jumlah--;
                cout<<"Dibatalkan\n";
                return;
            }
        }
    }

    void update() {
        string no;
        cout<<"No Antrian: "; cin>>no;

        for(int i=0;i<jumlah;i++) {
            if(antrian[i].noAntrian==no) {
                cout<<"Nama baru: ";
                cin>>antrian[i].nama;
                return;
            }
        }
    }

    void sortingNama() {
        for(int i=0;i<jumlah-1;i++) {
            for(int j=i+1;j<jumlah;j++) {
                if(antrian[i].nama > antrian[j].nama) {
                    swap(antrian[i], antrian[j]);
                }
            }
        }
        cout<<"Diurutkan berdasarkan nama\n";
    }

    void tampilRiwayat() {
        for(int i=0;i<selesai;i++) {
            cout<<riwayat[i].p.nama<<" selesai\n";
        }
    }

    void menu() {
        int pilih;
        while(true) {
            cout<<"\n1.Daftar 2.Tampil 3.Panggil 4.Selesai\n";
            cout<<"5.Batal 6.Update 7.Sort 8.Riwayat 0.Keluar\n";
            cout<<"Pilih: ";
            cin>>pilih;

            switch(pilih) {
                case 1: daftar(); break;
                case 2: tampil(); break;
                case 3: panggil(); break;
                case 4: selesaiPasien(); break;
                case 5: batal(); break;
                case 6: update(); break;
                case 7: sortingNama(); break;
                case 8: tampilRiwayat(); break;
                case 0: return;
            }
        }
    }
};

int main() {
    Sistem s;
    s.menu();
}
