#include <iostream>
#include <fstream>
#include <string>
using namespace std;

// STRUCT untuk data jadwal kereta api
struct KeretaApi {
    string kodeKereta;
    string namaKereta;
    string asal;
    string tujuan;
    string jamBerangkat;
    string jamTiba;
    int hargaTiket;
};

const int MAX_DATA = 100;
KeretaApi dataKereta[MAX_DATA];
int jumlahData = 0;

// Fungsi untuk menu
void tampilkanMenu() {
    cout << "\n========================================\n";
    cout << "   SISTEM MANAJEMEN JADWAL KERETA API   \n";
    cout << "========================================\n";
    cout << "1. Input Data Jadwal Kereta\n";
    cout << "2. Tampilkan Semua Data\n";
    cout << "3. Sorting - Bubble Sort (Berdasarkan Harga)\n";
    cout << "4. Sorting - Shell Sort (Berdasarkan Kode)\n";
    cout << "5. Searching Data\n";
    cout << "6. Simpan Data ke File\n";
    cout << "7. Load Data dari File\n";
    cout << "8. Keluar\n";
    cout << "========================================\n";
    cout << "Pilih menu (1-8): ";
}

// Fungsi input data (Bisa banyak data sekaligus)
void inputData() {
    int jumlahInput;
    cout << "\nMau input berapa data? ";
    cin >> jumlahInput;
    cin.ignore(); // Bersihkan buffer setelah input angka
    
    if (jumlahInput <= 0) {
        cout << "Jumlah input tidak valid!\n";
        return;
    }

    // Validasi batas maksimum array
    if (jumlahData + jumlahInput > MAX_DATA) {
        cout << "Gagal! Slot data tidak cukup. Sisa slot kosong: " << (MAX_DATA - jumlahData) << endl;
        return;
    }
    
    for (int i = 0; i < jumlahInput; i++) {
        cout << "\n--- Input Data Jadwal Kereta ke-" << (i + 1) << " ---\n";
        cout << "Kode Kereta (contoh: KA001): ";
        getline(cin, dataKereta[jumlahData].kodeKereta);
        
        cout << "Nama Kereta (contoh: Argo Bromo): ";
        getline(cin, dataKereta[jumlahData].namaKereta);
        
        cout << "Stasiun Asal: ";
        getline(cin, dataKereta[jumlahData].asal);
        
        cout << "Stasiun Tujuan: ";
        getline(cin, dataKereta[jumlahData].tujuan);
        
        cout << "Jam Berangkat (HH:MM): ";
        getline(cin, dataKereta[jumlahData].jamBerangkat);
        
        cout << "Jam Tiba (HH:MM): ";
        getline(cin, dataKereta[jumlahData].jamTiba);
        
        cout << "Harga Tiket (Rp): ";
        cin >> dataKereta[jumlahData].hargaTiket;
        cin.ignore(); // Bersihkan newline setelah input integer
        
        jumlahData++; // Increment jumlah data global
    }
    
    cout << "\nSemua (" << jumlahInput << ") data berhasil ditambahkan!\n";
}

void tampilkanData() {
    if (jumlahData == 0) {
        cout << "\nBelum ada data!\n";
        return;
    }

    cout << "\n========================================================\n";
    cout << "           DAFTAR JADWAL KERETA API\n";
    cout << "========================================================\n";

    for (int i = 0; i < jumlahData; i++) {
        cout << "\nData Ke-" << i + 1 << endl;
        cout << "Kode Kereta   : " << dataKereta[i].kodeKereta << endl;
        cout << "Nama Kereta   : " << dataKereta[i].namaKereta << endl;
        cout << "Asal          : " << dataKereta[i].asal << endl;
        cout << "Tujuan        : " << dataKereta[i].tujuan << endl;
        cout << "Jam Berangkat : " << dataKereta[i].jamBerangkat << endl;
        cout << "Jam Tiba      : " << dataKereta[i].jamTiba << endl;
        cout << "Harga Tiket   : Rp " << dataKereta[i].hargaTiket << endl;
        cout << "--------------------------------------------------------\n";
    }
}

// BUBBLE SORT - Berdasarkan Harga Tiket (Ascending)
void bubbleSort() {
    if (jumlahData == 0) {
        cout << "\nBelum ada data untuk di-sort!\n";
        return;
    }
    
    cout << "\n--- Bubble Sort (Berdasarkan Harga - Termurah ke Termahal) ---\n";
    
    for (int i = 0; i < jumlahData - 1; i++) {
        for (int j = 0; j < jumlahData - i - 1; j++) {
            if (dataKereta[j].hargaTiket > dataKereta[j + 1].hargaTiket) {
                KeretaApi temp = dataKereta[j];
                dataKereta[j] = dataKereta[j + 1];
                dataKereta[j + 1] = temp;
            }
        }
    }
    
    cout << "Data berhasil di-sort dengan Bubble Sort!\n";
    tampilkanData();
}

// SHELL SORT - Berdasarkan Kode Kereta
void shellSort() {
    if (jumlahData == 0) {
        cout << "\nBelum ada data untuk di-sort!\n";
        return;
    }
    
    cout << "\n--- Shell Sort (Berdasarkan Kode Kereta) ---\n";
    
    for (int gap = jumlahData / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < jumlahData; i++) {
            KeretaApi temp = dataKereta[i];
            int j;
            
            for (j = i; j >= gap && dataKereta[j - gap].kodeKereta > temp.kodeKereta; j -= gap) {
                dataKereta[j] = dataKereta[j - gap];
            }
            dataKereta[j] = temp;
        }
    }
    
    cout << "Data berhasil di-sort dengan Shell Sort!\n";
    tampilkanData();
}

// SEARCHING - Linear Search berdasarkan Kode/Nama Kereta
void searchingData() {
    if (jumlahData == 0) {
        cout << "\nBelum ada data untuk dicari!\n";
        return;
    }

    string cari;
    cout << "\n--- Searching Data Kereta ---\n";
    cout << "Masukkan Kode atau Nama Kereta yang dicari: ";
    getline(cin, cari);

    bool ditemukan = false;

    cout << "\n========================================================\n";
    cout << "                 HASIL PENCARIAN\n";
    cout << "========================================================\n";

    for (int i = 0; i < jumlahData; i++) {
        if (dataKereta[i].kodeKereta.find(cari) != string::npos ||
            dataKereta[i].namaKereta.find(cari) != string::npos) {

            cout << "\nData Ditemukan\n";
            cout << "Kode Kereta   : " << dataKereta[i].kodeKereta << endl;
            cout << "Nama Kereta   : " << dataKereta[i].namaKereta << endl;
            cout << "Asal          : " << dataKereta[i].asal << endl;
            cout << "Tujuan        : " << dataKereta[i].tujuan << endl;
            cout << "Jam Berangkat : " << dataKereta[i].jamBerangkat << endl;
            cout << "Jam Tiba      : " << dataKereta[i].jamTiba << endl;
            cout << "Harga Tiket   : Rp " << dataKereta[i].hargaTiket << endl;
            cout << "--------------------------------------------------------\n";

            ditemukan = true;
        }
    }

    if (!ditemukan) {
        cout << "\nData tidak ditemukan!\n";
    }

    cout << "========================================================\n";
}

// Simpan data ke file
void simpanKeFile() {
    ofstream file;
    file.open("jadwal_kereta.txt", ios::out | ios::trunc);
    
    if (!file.is_open()) {
        cout << "\nGagal membuka file untuk menyimpan!\n";
        return;
    }
    
    for (int i = 0; i < jumlahData; i++) {
        file << dataKereta[i].kodeKereta << "\n";
        file << dataKereta[i].namaKereta << "\n";
        file << dataKereta[i].asal << "\n";
        file << dataKereta[i].tujuan << "\n";
        file << dataKereta[i].jamBerangkat << "\n";
        file << dataKereta[i].jamTiba << "\n";
        file << dataKereta[i].hargaTiket << "\n";
    }
    
    file.close();
    cout << "\nData berhasil disimpan ke file 'jadwal_kereta.txt'!\n";
}

// Load data dari file
void loadDariFile() {
    ifstream file;
    file.open("jadwal_kereta.txt", ios::in);
    
    if (!file.is_open()) {
        cout << "\nFile tidak ditemukan atau gagal dibuka!\n";
        return;
    }
    
    jumlahData = 0;
    
    while (jumlahData < MAX_DATA && getline(file, dataKereta[jumlahData].kodeKereta)) {
        getline(file, dataKereta[jumlahData].namaKereta);
        getline(file, dataKereta[jumlahData].asal);
        getline(file, dataKereta[jumlahData].tujuan);
        getline(file, dataKereta[jumlahData].jamBerangkat);
        getline(file, dataKereta[jumlahData].jamTiba);
        
        if (file >> dataKereta[jumlahData].hargaTiket) {
            file.ignore(); 
            jumlahData++;
        }
    }
    
    file.close();
    cout << "\nData berhasil dimuat dari file! Total data: " << jumlahData << "\n";
}

// Program utama
int main() {
    int pilihan;
    
    do {
        tampilkanMenu();
        cin >> pilihan;
        cin.ignore(); // Bersihkan buffer pilihan menu
        
        switch (pilihan) {
            case 1:
                inputData();
                break;
            case 2:
                tampilkanData();
                break;
            case 3:
                bubbleSort();
                break;
            case 4:
                shellSort();
                break;
            case 5:
                searchingData();
                break;
            case 6:
                simpanKeFile();
                break;
            case 7:
                loadDariFile();
                break;
            case 8:
                cout << "\nTerima kasih telah menggunakan program ini!\n";
                break;
            default:
                cout << "\nPilihan tidak valid!\n";
        }
    } while (pilihan != 8);
    
    return 0;
}
