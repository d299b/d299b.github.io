// C program to build the complete 
// snake game 
#define _CRT_SECURE_NO_WARNINGS
#include <conio.h> 
#include <stdio.h> 
#include <stdlib.h> 
#include <string.h>
#include <io.h> 
#include <Windows.h>

int i, j, height = 20, width = 20;
int gameover, score;
int x, y, fruitx, fruity, rot_fr_x, rot_fr_y, flag;
int length;

// x, y 좌표를 감지하는 방식이 int값에서 구조체로 변경했기 때문에
//x, y 좌표를 감지하려면 (구조체 이름).x 같은 식으로 접근해야함
typedef struct {
	int x, y;
} Point;	//gotoxy에서 사용하기 위한 구조체

typedef struct rank_info {
	char* name;
	int score;
	struct rank_info* next;
}RNKINFO;

typedef struct list {
	struct rank_info* cur;
	struct rank_info* head;
	struct rank_info* tail;
}LINKEDLIST;

Point snake[400]; // 뱀 몸 길이 최대 400

void game_draw();	// 함수 선언먼저 해두기(게임실행함수)
void rank_input();
void printScore(LINKEDLIST* lnk);
void insertData(LINKEDLIST* lnk, char* name, int score);
void deleteLastNode(LINKEDLIST* lnk);
void RankingShow(void);

void SetColor(int color) {			//컬러 지정(크롤링)
	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

void CursorView(char show)			//커서가 안보이게 해주는 함수(크롤링)
{
	HANDLE hConsole;
	CONSOLE_CURSOR_INFO ConsoleCursor;

	hConsole = GetStdHandle(STD_OUTPUT_HANDLE);

	ConsoleCursor.bVisible = show;
	ConsoleCursor.dwSize = 1;

	SetConsoleCursorInfo(hConsole, &ConsoleCursor);
}

void gotoxy(int x, int y) {			//UI 구현에 사용하는 포지션 이동(크롤링)
	COORD pos = { x, y };
	SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), pos);
}

void start_screen_print() {	//시작화면 출력
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	gotoxy(21, 4); printf("SNAKE 게임 ");
	gotoxy(12, 7); printf("계속하려면 아무키나 눌러주세요.");


	int start_input = 0;
	while (1) {
		start_input = _getch();		//아무 입력을 받아옵니다
		if (start_input >= 1) {		//무슨 입력을 받던 간에 start_input은 1보다 커지게 되있습니다
			system("cls");
			start_input = 0;			//i 초기화
			break;
		}
	}
}

void quit_game() {
	system("cls");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	gotoxy(19, 6); printf("게임을 종료합니다.");
	gotoxy(25, 10);
	gameover = 1;
	exit(0);
}

void main_print() {
	CursorView(0);
	fflush(stdin);
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	gotoxy(7, 4); printf("새로 하기");
	gotoxy(21, 4); printf("랭킹 보기");
	gotoxy(35, 4); printf("게임 종료");
	gotoxy(6, 7); printf("SPACE : 메뉴선택      방향키 : 메뉴 이동");

	int POS = 0;
	while (1) {
		if (GetAsyncKeyState(VK_LEFT))
			if (POS == 0) POS = 2;
			else POS -= 1;
		else if (GetAsyncKeyState(VK_RIGHT))
			if (POS == 2 || POS == 4) POS = 0;
			else POS += 1;
		else if (GetAsyncKeyState(VK_SPACE)) {	//스페이스바를 눌렀을 때
			if (POS == 0) {
				system("cls");
				game_draw();
				break;
			}
			else if (POS == 1) {
				system("cls");
				RankingShow();
				break;
			}
			else if (POS == 2) {
				quit_game();
				gameover = 1;
				break;
			}
		}
		switch (POS) {
		case 0:
			SetColor(11);
			gotoxy(7, 4); printf("새로 하기");
			SetColor(15);
			gotoxy(21, 4); printf("랭킹 보기");
			gotoxy(35, 4); printf("게임 종료");
			break;

		case 1:
			SetColor(15);
			gotoxy(7, 4); printf("새로 하기");
			SetColor(11);
			gotoxy(21, 4); printf("랭킹 보기");
			SetColor(15);
			gotoxy(35, 4); printf("게임 종료");
			break;

		case 2:
			SetColor(15);
			gotoxy(7, 4); printf("새로 하기");
			gotoxy(21, 4); printf("랭킹 보기");
			SetColor(11);
			gotoxy(35, 4); printf("게임 종료");
			SetColor(15);
			break;

		default: break;
		}
		Sleep(100);
	}
}

void gameover_print() {	//게임 오버시 최종점수 저장
	system("cls");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	gotoxy(21, 2); printf("GAME OVER");
	gotoxy(19, 4); printf("최종 점수 : %d", score);
	gotoxy(12, 6); printf("계속하려면 X 키를 눌러주세요.");
	gotoxy(10, 7); printf("랭크를 등록하려면 C키를 눌러주세요.");


	while (1) {
		int go_input = 0;
		go_input = _getch();
		if (go_input == 120) {
			system("cls");
			main_print();
		}
		if (go_input == 99) {
			system("cls");
			rank_input();
			main_print();
		}
	}
}

void rank_input(void) {
	char player_name[50];
	system("cls");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■                                                   ■");
	puts("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
	gotoxy(20, 4); printf("이름을 입력해주세요 : ");
	scanf("%s", player_name);

	// 메모장 열기
	FILE* file = fopen("Rank_List.txt", "a");
	if (file == NULL) {
		printf("Error opening file!");
		return;
	}

	// 입력받은 이름과 게임 점수 입력하기
	fprintf(file, "%s\t", player_name);
	fprintf(file, "%d\n", score);

	fclose(file);

	return;
}
//랭크 보여주는 장면(콘솔)
void RankingShow(void) {
	system("mode con: cols=50 lines=30");
	system("title 랭킹 확인");

	FILE* fp = fopen("Rank_List.txt", "rt");
	if (fp == NULL) {
		MessageBox(NULL, TEXT("The file does not exist."), NULL, NULL);
		return;
	}

	LINKEDLIST* lnk = (LINKEDLIST*)malloc(sizeof(LINKEDLIST));
	lnk->cur = NULL;
	lnk->head = NULL;
	lnk->tail = NULL;

	char tempName[50];
	int tempScore;
	while (fscanf(fp, "%s %d", tempName, &tempScore) == 2) {
		insertData(lnk, tempName, tempScore);
	}

	fclose(fp);

	printScore(lnk);

	while (lnk->head != NULL) {
		deleteLastNode(lnk);
	}

	Sleep(1000);

	return;
}
//콘솔에 내림차순으로 print하기
void printScore(LINKEDLIST* lnk) {
	int cnt = 0;
	RNKINFO* cur;
	cur = lnk->head;

	while (cur != NULL) {
		cnt++;
		cur = cur->next;
	}

	RNKINFO** ptr = (RNKINFO**)malloc(sizeof(RNKINFO*) * cnt);
	int i, j;
	RNKINFO* temp;

	for (i = 0, cur = lnk->head; i < cnt; i++) {
		ptr[i] = cur;
		if (cur == NULL)
			break;
		cur = cur->next;
	}

	for (i = 0; i < cnt - 1; i++) {
		for (j = i + 1; j < cnt; j++) {
			if (ptr[i]->score <= ptr[j]->score) {
				temp = ptr[i];
				ptr[i] = ptr[j];
				ptr[j] = temp;
			}
		}
	}

	for (i = 0; i < cnt; i++) {
		printf("%d. Name: %s \t\t Score: %d\n", i + 1, ptr[i]->name, ptr[i]->score);
	}

	free(ptr);
}
//메모장 데이터를 노드에 입력하기
void insertData(LINKEDLIST* lnk, char* name, int score) {
	RNKINFO* newRNK = (RNKINFO*)malloc(sizeof(RNKINFO));
	newRNK->name = (char*)malloc(sizeof(char) * (strlen(name) + 1));
	strcpy(newRNK->name, name);
	newRNK->score = score;
	newRNK->next = NULL;

	if (lnk->head == NULL && lnk->tail == NULL) {
		lnk->head = lnk->tail = newRNK;
	}
	else {
		lnk->tail->next = newRNK;
		lnk->tail = newRNK;
	}
	return;
}

// 마지막 노드 지우기
void deleteLastNode(LINKEDLIST* lnk) {
	if (lnk->head == NULL) {
		return;  // No nodes to delete
	}
	else if (lnk->head == lnk->tail) {
		// Only one node in the list
		free(lnk->head->name);
		free(lnk->head);
		lnk->head = lnk->tail = NULL;
	}
	else {
		RNKINFO* cur = lnk->head;
		while (cur->next != lnk->tail) {
			cur = cur->next;
		}
		cur->next = NULL;
		free(lnk->tail->name);
		free(lnk->tail);
		lnk->tail = cur;
	}
	return;
}

// Function to generate the fruit 
// within the boundary 
void setup()
{
	gameover = 0;
	length = 2; // 초기 몸 길이
	flag = 0; // 초기 방향

	// Stores height and width 
	snake[0].x = width / 2; // 초기 뱀 x위치
	snake[0].y = height / 2; // 초기 뱀 y위치

	fruitx = 0;
	while (fruitx == 0) {
		fruitx = rand() % 19;	//이하 rand 전부 19로 수정(벽에 생성되는걸 막기위함)
	}

	fruity = 0;
	while (fruity == 0) {
		fruity = rand() % 19;
	}

	rot_fr_x = 0;
	while (rot_fr_x == 0) {
		rot_fr_x = rand() % 19;
	}

	rot_fr_y = 0;
	while (rot_fr_y == 0) {
		rot_fr_y = rand() % 19;
	}


	score = 0;
}

// Function to draw the boundaries 
void draw()
{
	//system("cls");	화면을 계속 지우며 벽, 과일, 플레이어를 출력(이녀석이 화면을 깜빡이게 하는 원인)
	gotoxy(0, 0); //크롤링 해온 gotoxy 함수를 통해 화면을 지우지 않고 그대로 덮어씌우는 형식으로 작동하게 하여 화면 버퍼링을 없애준다.

	for (i = 0; i < height; i++) {
		for (j = 0; j < width; j++) {
			if (i == 0 || i == width - 1
				|| j == 0
				|| j == height - 1) {
				printf("#");
			}
			else {
				int tri = 0; // 몸을 출력했는지 확인용 트리거

				for (int r = 0; r < length; r++) { // 몸 길이 만큼 반복해서 몸 길이 만큼 출력
					if (snake[r].x == i && snake[r].y == j) { // 뱀 좌표와 같다면 출력
						printf("0");
						tri = 1; // 몸을 출력했으니 트리거가 1이 됨
						break;
					}
				}

				if (tri == 0) { // 몸이 출력되지 않았을 때 다른 걸 출력함
					if (i == fruitx
						&& j == fruity)
						printf("*");
					else if (i == rot_fr_x
						&& j == rot_fr_y)
						printf("?");
					else
						printf(" ");

				}


			}
		}
		printf("\n");
	}

	// Print the score after the 
	// game ends 
	printf("score = %d", score);
	printf("\n");
	printf("press X to quit the game");
}

// Function to take the input 
void input()
{
	if (_kbhit()) {
		switch (getch()) {
		case 'a':
			if (flag == 3) break; // 정반대 방향으로 이동하지 못 함
			flag = 1;
			break;
		case 's':
			if (flag == 4) break; // 정반대 방향으로 이동하지 못 함
			flag = 2;
			break;
		case 'd':
			if (flag == 1) break; // 정반대 방향으로 이동하지 못 함
			flag = 3;
			break;
		case 'w':
			if (flag == 2) break; // 정반대 방향으로 이동하지 못 함
			flag = 4;
			break;
		case 'x':
			gameover_print(score);
			break;
		}
	}
}

// Function for the logic behind 
// each movement 
void logic()
{
	Sleep(200);

	Point newHead = snake[0]; // 새로운 머리의 좌표를 구하기 위해 새로운 구조체를 선언함
	switch (flag) {
	case 1:
		newHead.y--; // 새로운 머리가 구조체니까 구조체로 접근함
		break;
	case 2:
		newHead.x++;
		break;
	case 3:
		newHead.y++;
		break;
	case 4:
		newHead.x--;
		break;
	default:
		break;
	}

	// 본인 몸에 박으면 게임오버
	if (flag != 0) { // 맨 처음 시작할 때 게임오버 되지 않게 하기 위함
		for (int i = 0; i < length; i++) { // 현재 몸 길이 만큼 반복
			if (snake[i].x == newHead.x && snake[i].y == newHead.y) { // 현재 뱀 위치(몸통)과 새로운 머리와 좌표가 같으면 게임오버
				gameover_print(score);
				return;
			}
		}
	}
	//몸이 몸 길이 만큼 한 칸씩 뒤로 밀림
	for (int i = length; i > 0; i--) {
		snake[i] = snake[i - 1];
	}

	//머리는 새로 갱신된 머리로 바뀜
	snake[0] = newHead;

	// 각종 판정은 머리를 기준으로 하기 때문에 newHead를 기준으로 함
	// If the game is over (벽에 박을떄)
	if (newHead.x < 0 || newHead.x > height
		|| newHead.y < 0 || newHead.y > width)
		gameover_print(score);

	// If snake reaches the fruit 
	// then update the score (정상적인 과일섭취)
	if (newHead.x == fruitx && newHead.y == fruity) {
		length += 1; // 과일을 먹으면 몸 길이가 길어짐
		fruitx = 0;
		while (fruitx == 0) {
			fruitx = rand() % 19;	//20으로 하니까 자꾸 벽이랑 겹쳐서 생성됨
		}

		fruity = 0;
		while (fruity == 0) {
			fruity = rand() % 19;	//20으로 하니까 자꾸 벽이랑 겹쳐서 생성됨
		}

		score += 10;
	}
	//썩은 과일 섭취
	if (newHead.x == rot_fr_x && newHead.y == rot_fr_y) {
		length += 3; // 썩은 과일은 몸길이를 훨씬 늘어나게함
		rot_fr_x = 0;
		while (rot_fr_x == 0) {
			rot_fr_x = rand() % 19;	//20으로 하니까 자꾸 벽이랑 겹쳐서 생성됨
		}

		rot_fr_y = 0;
		while (rot_fr_y == 0) {
			rot_fr_y = rand() % 19;	//20으로 하니까 자꾸 벽이랑 겹쳐서 생성됨
		}

		score -= 10;
	}
}

void game_draw() {
	setup();
	while (1) {
		// Function Call 
		draw();
		input();
		logic();
	}
};

void main()
{

	start_screen_print();
	main_print();

}
