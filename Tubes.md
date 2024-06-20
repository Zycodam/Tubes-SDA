```C++
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

// Struktur data untuk item menu // Linked List
struct MenuItem {
    string nama; // Nama item menu
    int harga; // Harga item menu
    MenuItem* next; // Pointer ke item menu berikutnya dalam linked list
};

// Struktur data untuk pesanan
struct Pesanan {
    MenuItem* item; // Pointer ke item menu yang dipesan
    int jumlah; // Jumlah pesanan
    Pesanan* next; // Pointer ke pesanan berikutnya dalam linked list
};

// Fungsi untuk menghitung total belanja dari daftar pesanan
int hitungTotalBelanja(Pesanan* head) {
    int total = 0;
    while (head != nullptr) {
        total += head->jumlah * head->item->harga; // Menghitung total untuk setiap pesanan
        head = head->next; // Beralih ke pesanan berikutnya
    }
    return total; // Mengembalikan total belanja
}

// Fungsi untuk menampilkan daftar pesanan saat ini
void tampilkanPesananSaatIni(Pesanan* head) {
    cout << "\nPesanan Saat Ini:\n";
    while (head != nullptr) {
        cout << head->item->nama << " x " << head->jumlah << " = Rp" << (head->jumlah * head->item->harga) << endl; // Menampilkan detail pesanan
        head = head->next; // Beralih ke pesanan berikutnya
    }
    cout << endl;
}

// Fungsi untuk membuat pesanan baru
Pesanan* buatPesanan(MenuItem* item, int jumlah) {
    Pesanan* newOrder = new Pesanan; // Membuat objek pesanan baru
    newOrder->item = item; // Menetapkan item menu
    newOrder->jumlah = jumlah; // Menetapkan jumlah pesanan
    newOrder->next = nullptr; // Inisialisasi next dengan nullptr
    return newOrder; // Mengembalikan pointer ke pesanan baru
}

// Fungsi untuk menampilkan menu
void tampilkanMenu(MenuItem* head) {
    cout << "Pilihan Billiard :\n" << endl;
    int nomor = 1;
    while (head != nullptr) {
        cout << nomor << ". " << head->nama << " - Rp" << head->harga << endl; // Menampilkan item menu dengan nomor urut
        head = head->next; // Beralih ke item menu berikutnya
        nomor++;
    }
    cout << endl;
}

// Fungsi untuk membuat item menu baru
MenuItem* buatMenuItem(string nama, int harga) {
    MenuItem* newItem = new MenuItem; // Membuat objek menu item baru
    newItem->nama = nama; // Menetapkan nama item menu
    newItem->harga = harga; // Menetapkan harga item menu
    newItem->next = nullptr; // Inisialisasi next dengan nullptr
    return newItem; // Mengembalikan pointer ke item menu baru
}

// Fungsi untuk memilih item menu berdasarkan nomor urut
MenuItem* pilihMenu(MenuItem* head, int pilihan) {
    int index = 1;
    while (head != nullptr) {
        if (index == pilihan) {
            return head; // Mengembalikan item menu yang sesuai dengan pilihan
        }
        head = head->next; // Beralih ke item menu berikutnya
        index++;
    }
    return nullptr; // Mengembalikan nullptr jika tidak ditemukan
}

// Fungsi untuk menambahkan pesanan baru ke daftar pesanan
void tambahkanPesanan(Pesanan*& head, Pesanan* newOrder) {
    if (head == nullptr) {
        head = newOrder; // Jika daftar pesanan kosong, tambahkan sebagai pesanan pertama
    } else {
        Pesanan* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next; // Beralih ke pesanan terakhir dalam daftar
        }
        temp->next = newOrder; // Menambahkan pesanan baru ke akhir daftar
    }
}

// MAIN FUNCTION // Array
int main() {
    MenuItem* menuHead = nullptr; // Pointer ke kepala linked list menu
    MenuItem* item1 = buatMenuItem("Biliard per-jam", 30000); // Membuat item menu pertama
    MenuItem* item2 = buatMenuItem("Ruang VIP", 40000); // Membuat item menu kedua
    MenuItem* item3 = buatMenuItem("Paket 3 jam", 50000); // Membuat item menu ketiga

    // Menyusun linked list menu
    item1->next = item2;
    item2->next = item3;
    menuHead = item1;

    Pesanan* pesananHead = nullptr; // Pointer ke kepala linked list pesanan
    string kodekasir, namakasir;
    cout << "Masukkan kode kasir: ";
    cin >> kodekasir; // Input kode kasir
    cout << "Masukkan nama kasir: ";
    cin >> namakasir; // Input nama kasir
    char lanjutPesan;

    cout << "\n==================== FAMOUS BILLIARD ====================\n" << endl;

    do {
        tampilkanMenu(menuHead); // Menampilkan menu
        tampilkanPesananSaatIni(pesananHead); // Menampilkan pesanan saat ini

        int pilihanMenu;
        int jumlahPesan;
        cout << "\n======================================================\n" << endl;
        cout << "Pilih menu (1-3): ";
        cin >> pilihanMenu; // Input pilihan menu

        MenuItem* itemDipilih = pilihMenu(menuHead, pilihanMenu); // Memilih item menu berdasarkan input
        if (itemDipilih == nullptr) {
            cout << "Pilihan tidak valid. Silakan pilih lagi.\n";
            continue; // Jika pilihan tidak valid, ulangi loop
        }

        cout << "Jumlah pesanan untuk menu " << itemDipilih->nama << ": ";
        cin >> jumlahPesan; // Input jumlah pesanan

        if (jumlahPesan < 0) {
            cout << "Jumlah pesanan tidak valid. Silakan pilih lagi.\n";
            continue; // Jika jumlah pesanan tidak valid, ulangi loop
        }

        Pesanan* newOrder = buatPesanan(itemDipilih, jumlahPesan); // Membuat pesanan baru
        tambahkanPesanan(pesananHead, newOrder); // Menambahkan pesanan baru ke daftar pesanan

        cout << "\nApakah anda ingin memesan lagi? (y/n): ";
        cin >> lanjutPesan; // Input apakah ingin memesan lagi

    } while (lanjutPesan == 'y' || lanjutPesan == 'Y'); // Ulangi jika jawabannya 'y' atau 'Y'

    cout << "\n=================== FAMOUS BILLIARD =====================";
    cout << "\n================== STRUK PEMBAYARAN =====================\n";
    cout << "\nKode Kasir: " << kodekasir << endl;
    cout << "Nama Kasir: " << namakasir << endl;
    tampilkanPesananSaatIni(pesananHead); // Menampilkan struk pesanan

    int totalBelanja = hitungTotalBelanja(pesananHead); // Menghitung total belanja
    cout << "Total Pembelian = Rp" << totalBelanja << "\n";
    cout << "\n======================================================\n" << endl;

    double nominalUang;
    cout << "Masukkan nominal uang yang diberikan pelanggan: Rp";
    cin >> nominalUang; // Input nominal uang yang diberikan pelanggan

    if (nominalUang < totalBelanja) {
        cout << "Uang yang diberikan kurang. Harap bayar sesuai nominal.\n"; // Jika uang kurang, minta bayar sesuai nominal
    } else {
        double kembalian = nominalUang - totalBelanja;
        cout << fixed << setprecision(0);
        cout << "Kembalian: Rp" << kembalian << "\n"; // Menampilkan kembalian
    }

    cout << "\nTerima kasih atas kunjungan anda di Famous Billiard!\n";

    // Menghapus memori yang terhubung ke menu
    while (menuHead != nullptr) {
        MenuItem* temp = menuHead;
        menuHead = menuHead->next;
        delete temp; // Menghapus setiap item menu dari memori
    }
    // Menghapus memori yang terhubung ke pesanan
    while (pesananHead != nullptr) {
        Pesanan* temp = pesananHead;
        pesananHead = pesananHead->next;
        delete temp; // Menghapus setiap pesanan dari memori
    }

    return 0; // Mengakhiri program
}
```
