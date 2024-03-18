#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_HANG_HOA 100

struct Hanghoa {
	char Mahang[20];
	char Tenhang[50];
	char DVT[50];
	int soluong;
	struct Hanghoa* next;
};

typedef struct Hanghoa Hanghoa;

struct KhoHang {
	struct Hanghoa* Head, * Tail;
};

typedef struct Khohang Khohang;

struct Hanghoa* creatNewHanghoa(char Mahang[20], char Tenhang[50], char DVT[50], int soluong) {
	struct Hanghoa* newHanghoa = (Hanghoa*)malloc(sizeof(Hanghoa));
	if (newHanghoa == NULL) {
		printf("Memory allocation failed");
		return NULL;
	}
	strcpy_s(newHanghoa->Mahang, Mahang);
	strcpy_s(newHanghoa->Tenhang, Tenhang);
	strcpy_s(newHanghoa->DVT, DVT);
	newHanghoa->soluong = soluong;
	newHanghoa->next = NULL;
	return newHanghoa;
}

int isEmpty(KhoHang* khohang) {
	if (khohang->Head == NULL) {
		return 1;
	}
	else {
		return 0;
	}
}

//Thêm hàng hóa vào kho
void AddHangHoa(KhoHang* khohang, char Mahang[20], char Tenhang[50], char DVT[50], int soluong) {
	Hanghoa* newHangHoa = creatNewHanghoa(Mahang, Tenhang, DVT, soluong);
	if (newHangHoa == NULL) {
		printf("Khong the them hang hoa vao kho");
		return;
	}
	if (isEmpty(khohang)) {
		khohang->Head = khohang->Tail = newHangHoa;
	}
	else {
		khohang->Tail->next = newHangHoa;
		khohang->Tail = newHangHoa;
	}
}

//Xóa hàng hóa ra khỏi kho
void deleteHangHoa(KhoHang* khohang) {
	if (khohang == NULL || isEmpty(khohang)) {
		printf("Kho hang rong khong the xoa\n");
		return;
	}
	Hanghoa* tmp = khohang->Head;
	khohang->Head = khohang->Head->next;
	free(tmp);
}

//Lấy thông tin hàng hóa đầu ra xem
Hanghoa* getHead(KhoHang* khohang) {
	if (isEmpty(khohang)) {
		printf("Hang doi rong,khong co hang hoa xuat ra ma hinh\n");
		return NULL;
	}
	else {
		return khohang->Head;
	}
}

//Hiển thị danh sách hàng hóa trong kho
void displaykhohang(KhoHang* khohang) {
	if (isEmpty(khohang)) {
		printf("Khong co hang hoa de xuat ra ma hinh\n");
		return;
	}
	else {
		Hanghoa* current = khohang->Head;
		printf("Danh sach hang hoa:\n");
		while (current != NULL) {
			printf("Ma hang: %s, Ten hang: %s, Don vi tinh: %s, So luong: %d\n", current->Mahang, current->Tenhang, current->DVT, current->soluong);
			current = current->next;
		}
	}
}

int main() {
	struct KhoHang myKhoHang;
	myKhoHang.Head = myKhoHang.Tail = NULL;
	int lc, n;
	char Mahang[20];
	char Tenhang[50];
	char DVT[50];
	int soluong;
	while (1) {
		printf("1. Them hang hoa vao kho\n");
		printf("2. Xuat hang hoa\n");
		printf("3. Xuat thong tin hang hoa dau ra man hinh\n");
		printf("4. Xuat toan bo hang hoa ra man hinh\n");
		printf("0. Thoat!\n");
		printf("Moi ban lua chon: ");
		scanf_s("%d", &lc);
		if (lc == 1) {
  	// Nhập số lượng mặt hàng
		printf("Nhap so luong mat hang (n >= 10): ");
		scanf_s("%d", &n);
		// Kiểm tra nếu n không đủ lớn
		if (n < 10) {
			printf("So luong mat hang khong hop le. Phai nhap so luong lon hon hoac bang 10.\n");
			return 1;
		}
			for (int i = 0; i < n; i++) {
				printf("Nhap thong tin cho mat hang thu %d\n ", i + 1);
				printf("Nhap ma hang: ");
				scanf_s("%s", Mahang, (unsigned)_countof(Mahang));
				printf("Nhap ten hang: ");
				scanf_s("%s", Tenhang, (unsigned)_countof(Tenhang));
				printf("Nhap Don Vi Tinh: ");
				scanf_s("%s", DVT, (unsigned)_countof(DVT));
				printf("Nhap so luong: ");
				scanf_s("%d", &soluong);
				AddHangHoa(&myKhoHang, Mahang, Tenhang, DVT, soluong);
			}
		}

		else if (lc == 2) {
			printf("Hang hoa xuat ra kho: Ma hang %s, Ten hang %s, Don vi tinh %s, So luong %d", myKhoHang.Head->Mahang, myKhoHang.Head->Tenhang, myKhoHang.Head->DVT, myKhoHang.Head->soluong);
			deleteHangHoa(&myKhoHang);
		}
		else if (lc == 3) {
			printf("Hang hoa dau tien la: Ma hang %s, Ten hang %s, Don vi tinh %s, So luong %d", myKhoHang.Head->Mahang, myKhoHang.Head->Tenhang, myKhoHang.Head->DVT, myKhoHang.Head->soluong);
			getHead(&myKhoHang);
		}
		else if (lc == 4) {
			displaykhohang(&myKhoHang);      
		}
		else {
			break;
		}
	}
	return 0;
}
