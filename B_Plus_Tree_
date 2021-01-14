#include <stdio.h>

#define DEGREE 2

typedef struct node
{
	int key[2 * DEGREE - 1];
	struct node* child[2 * DEGREE];
	int* data[2 * DEGREE - 1];
	int leaf;
	int n;
	struct node* next;
}node;

typedef struct B_Plus_Tree
{
	node* root;
}B_Plus_Tree;

void B_Plus_Tree_Create(B_Plus_Tree* T);
void B_Plus_Tree_Insert(B_Plus_Tree* T, int k, int d);
void B_Plus_Tree_Insert_Nonfull(node* x, int k, int d);
void B_Plus_Tree_Split_Leaf(node* x, int i);
void B_Plus_Tree_Split_Nonleaf(node* x, int i);
void B_Plus_Tree_Delete(B_Plus_Tree* T, int k);
void B_Plus_Tree_Delete_main(node* T, int k);


void B_Plus_Tree_Traverse(B_Plus_Tree* T);
void B_Plus_Tree_Traverse_main(node* x, int h);


void main() {
	B_Plus_Tree* T = malloc(sizeof(B_Plus_Tree));
	if (T == NULL) {
		printf("memory allocation failed");
		return;
	}
	B_Plus_Tree_Create(T);
	for (int i = 1; i <= 10; i++) {
		B_Plus_Tree_Insert(T, i, i * 1000);
	}
	B_Plus_Tree_Traverse(T);
	for (int i = 1; i <= 10; i++) {
		B_Plus_Tree_Delete(T, 1);
	}

	return;
}

void B_Plus_Tree_Create(B_Plus_Tree* T) {
	node* x = malloc(sizeof(node));
	if (x == NULL) {
		printf("memory allocation failed");
		return;
	}
	x->n = 0;
	x->next = NULL;
	x->leaf = 1;
	T->root = x;
}

void B_Plus_Tree_Insert(B_Plus_Tree* T, int k, int d) {
	node* r = T->root;
	if (r->n == 2 * DEGREE - 1) {//root노드가 가득찬 경우
		node* s = malloc(sizeof(node));
		if (s == NULL) {
			printf("memory allocation failed");
			return;
		}
		s->leaf = 0;
		s->n = 0;
		s->next = NULL;
		s->child[0] = r;
		T->root = s;
		if (r->leaf == 1) {//root노드가 리프노드인 경우
			B_Plus_Tree_Split_Leaf(s, 0);
		}
		else {//root노드가 리프노드가 아닌 경우
			B_Plus_Tree_Split_Nonleaf(s, 0);
		}
		B_Plus_Tree_Insert_Nonfull(s, k, d);
	}
	else {//root노드에 여유공간이 있는 경우
		B_Plus_Tree_Insert_Nonfull(r, k, d);
	}
	return;
}

void B_Plus_Tree_Insert_Nonfull(node* x, int k, int d) {
	int i = 0;
	while (i < x->n && k > x->key[i]) {
		i++;
	}
	if (x->leaf == 1) {//x가 리프노드일 때,
		for (int j = x->n-1; j >i-1; j--) {
			x->key[j + 1] = x->key[j];
			x->data[j + 1] = x->data[j];
		}
		x->n++;
		x->key[i] = k;
		int* insert_data = malloc(sizeof(int));
		if (insert_data == NULL) {
			printf("memory allocation failed");
			return;
		}
		*insert_data = d;
		x->data[i] = insert_data;
	}
	else{//x가 리프노드가 아닐 때,
		node* y = x->child[i];
		if (y->n == DEGREE * 2 - 1) {
			if (y->leaf == 1) {//child가 리프노드인 경우
				B_Plus_Tree_Split_Leaf(x, i);
			}
			else {//child가 리프노드가 아닌 경우
				B_Plus_Tree_Split_Nonleaf(x, i);
			}
			if (x->key[i] < k) {
				i++;
			}
		}
		B_Plus_Tree_Insert_Nonfull(x->child[i], k, d);
	}
}


void B_Plus_Tree_Split_Leaf(node* x, int i) {
	node* y = x->child[i];
	node* z = malloc(sizeof(node));
	if (z == NULL) {
		printf("memory allocation failed");
		return;
	}
	z->leaf = y->leaf;
	z->next = y->next;
	y->next = z;
	for (int j = 0; j < DEGREE - 1; j++) {
		z->key[j] = y->key[j + DEGREE];
		z->data[j] = y->data[j + DEGREE];
	}
	y->n = DEGREE;
	z->n = DEGREE - 1;
	for (int j = x->n - 1; j > i - 1; j--) {
		x->key[j + 1] = x->key[j];
	}
	for (int j = x->n; j > i; j--) {
		x->child[j + 1] = x->child[j];
	}
	x->key[i] = y->key[DEGREE - 1];
	x->child[i + 1] = z;
	x->n++;
}


void B_Plus_Tree_Split_Nonleaf(node* x, int i) {
	node* y = x->child[i];
	node* z = malloc(sizeof(node));
	if (z == NULL) {
		printf("memory allocation failed");
		return;
	}
	z->leaf = y->leaf;
	for (int j = 0; j < DEGREE - 1; j++) {
		z->key[j] = y->key[j + DEGREE];
	}
	for (int j = 0; j < DEGREE; j++) {
		z->child[j] = y->child[j + DEGREE];
	}
	for (int j = x->n - 1; j > i - 1; j--) {
		x->key[j + 1] = x->key[j];
	}
	for (int j = x->n; j > i; j--) {
		x->child[j + 1] = x->child[j];
	}
	x->child[i + 1] = z;
	x->key[i] = y->key[DEGREE - 1];
	y->n = DEGREE - 1;
	z->n = DEGREE - 1;
	x->n++;
}

void B_Plus_Tree_Traverse(B_Plus_Tree* T) {
	node* r = T->root;
	printf("[printf b plus tree]\n");
	B_Plus_Tree_Traverse_main(r,0);
	printf("[end]\n\n");
}

void B_Plus_Tree_Traverse_main(node* x,int h) {
	printf("%d: ",h);
	if (x->leaf == 1) {
		for (int i = 0; i < x->n; i++) {
			printf("[%d %d]", x->key[i], *(x->data[i]));
		}
	}
	else {
		for (int i = 0; i < x->n; i++) {
			printf("%d ", x->key[i]);
		}
	}
	printf("\n");
	if (x->leaf == 0) {
		for (int i = 0; i < x->n + 1; i++) {
			B_Plus_Tree_Traverse_main(x->child[i], h + 1);
		}
	}
}


void B_Plus_Tree_Delete(B_Plus_Tree* T, int k) {
	node* r = T->root;
	if ((r->n == 1 && r->leaf==0) && ((r->child[0])->n == DEGREE - 1 && (r->child[1])->n == DEGREE - 1)) {
		//루트 노드의 키 개수가 1이고, 자식 노드들의 키 개수도 모두 DEGREE - 1이라면,
		node* y = r->child[0];
		node* z = r->child[1];
		if (y->leaf == 1) {//루트 노드의 자식 노드가 리프노드일 경우
			for (int j = 0; j < DEGREE - 1; j++) {
				y->key[j + DEGREE - 1] = z->key[j];
				y->data[j + DEGREE - 1] = z->data[j];
			}
			T->root = y;
			y->next = NULL;
			y->n = 2 * DEGREE - 2;
			free(r);
			free(z);
		}
		else{//루트 노드의 자식 노드가 리프노드가 아닐 경우
			y->key[DEGREE - 1] = r->key[0];
			for (int j = 0; j < DEGREE - 1; j++) {
				y->key[j + DEGREE] = z->key[j];
			}
			for (int j = 0; j < DEGREE; j++) {
				y->child[j + DEGREE] = z->child[j];
			}
			T->root = y;
			y->n = 2 * DEGREE - 1;
			free(r);
			free(z);
		}
		B_Plus_Tree_Delete_main(y, k);
	}
	else{
		B_Plus_Tree_Delete_main(r, k);
	}
}

void B_Plus_Tree_Delete_main(node* x, int k) {
	int i = 0;
	if (x->leaf == 1) {//타겟 노드가 리프 노드인 경우 >> 삭제하고 키와 데이터를 당겨주자
		while (1) {
			if (x->key[i] == k) {
				break;
			}
			i++;
		}
		for (int j = i; j < x->n - 1; j++) {
			x->key[j] = x->key[j + 1];
			x->data[j] = x->data[j + 1];
		}
		x->n--;
	}
	else {//타겟 노드가 리프노드가 아닌 경우
		while (i < x->n && k > x->key[i]) {
			i++;
		}
		node* my_way = x->child[i];
		if (my_way->n > DEGREE - 1) {	//내가 내려갈 곳에 key가 충분히 존재할 때
			B_Plus_Tree_Delete_main(my_way, k);
		}
		else {							//내가 내려갈 곳에 key가 충분히 존재하지 않을 때
			if (i == x->n) {					//타겟 노드가 형제 노드들 중 제일 오른쪽 노드인 경우
				node* left_c = x->child[i - 1];
				if (left_c->n > DEGREE - 1) {		//왼쪽 형제에게 빌릴 수 있다.
					if (left_c->leaf == 1) {				//left_c가 leaf노드인 경우
						for (int j = my_way->n - 1; j >= 0; j--) {
							my_way->key[j + 1] = my_way->key[j];
							my_way->data[j + 1] = my_way->data[j];
						}
						my_way->key[0] = left_c->key[left_c->n - 1];
						my_way->data[0] = left_c->data[left_c->n - 1];
						my_way->n++;
						left_c->n--;
						x->key[i - 1] = left_c->key[left_c->n - 1];
					}
					else {									//left_c가 leaf노드가 아닌 경우
						for (int j = my_way->n - 1; j >= 0; j--) {
							my_way->key[j + 1] = my_way->key[j];
						}
						for (int j = my_way->n; j >= 0; j--) {
							my_way->child[j + 1] = my_way->child[j];
						}
						my_way->key[0] = x->key[i - 1];
						my_way->child[0] = left_c->child[left_c->n];
						my_way->n++;
						x->key[i - 1] = left_c->key[left_c->n - 1];
						left_c->n--;
					}
				}
				else{								//왼쪽 형제에게 빌릴 수 없다. >> 왼쪽과 합치자
					if (left_c->leaf == 1) {				//left_c가 leaf노드인 경우
						for (int j = 0; j < my_way->n; j++) {
							left_c->key[j + DEGREE - 1] = my_way->key[j];
							left_c->data[j + DEGREE - 1] = my_way->data[j];
						}
						left_c->n = DEGREE * 2 - 2;
						x->n--;
						left_c->next = my_way->next;
						free(my_way);
						my_way = left_c;
					}
					else {									//left_c가 leaf노드가 아닌 경우
						left_c->key[DEGREE - 1] = x->key[i - 1];
						for (int j = 0; j < my_way->n; j++) {
							left_c->key[j + DEGREE] = my_way->key[j];
						}
						for (int j = 0; j < my_way->n + 1; j++) {
							left_c->child[j+DEGREE] = my_way->child[j];
						}
						left_c->n = 2 * DEGREE - 1;
						x->n--;
						free(my_way);
						my_way = left_c;
					}
				}
			}
			else {								//타겟 노드가 형제 노드들 중 제일 오른쪽 노드가 아닌 경우
				node* right_c = x->child[i + 1];
				if (right_c->n > DEGREE - 1) {		//오른쪽 형제에게 빌릴 수 있다.
					if (right_c->leaf == 1) {				//right_c가 leaf노드인 경우
						my_way->key[DEGREE - 1] = right_c->key[0];
						my_way->data[DEGREE - 1] = right_c->data[0];
						x->key[i] = right_c->key[0];
						my_way->n++;
						for (int j = 0; j < right_c->n - 1; j++) {
							right_c->key[j] = right_c->key[j + 1];
						}
						right_c->n--;
					}
					else {									//right_c가 leaf노드가 아닌 경우
						my_way->key[DEGREE - 1] = x->key[i];
						my_way->child[DEGREE] = right_c->child[0];
						my_way->n++;
						x->key[i] = right_c->key[0];
						for (int j = 0; j < right_c->n - 1; j++) {
							right_c->key[j] = right_c->key[j + 1];
						}
						for (int j = 0; j < right_c->n; j++) {
							right_c->child[j] = right_c->child[j + 1];
						}
						right_c->n--;
					}
				}
				else {								//오른쪽 형제에게 빌릴 수 없다. >> 오른쪽과 합치자
					if (right_c->leaf == 1) {				//right_c가 leaf노드인 경우
						for (int j = 0; j < DEGREE - 1; j++) {
							my_way->key[j + DEGREE - 1] = right_c->key[j];
							my_way->data[j + DEGREE - 1] = right_c->data[j];
						}
						my_way->n = 2 * DEGREE - 2;
						for (int j = i; j < x->n - 1; j++) {
							x->key[j] = x->key[j + 1];
							x->child[j + 1] = x->child[j + 1 + 1];
						}
						x->n--;
						my_way->next = right_c->next;
						free(right_c);
					}
					else {									//right_c가 leaf노드가 아닌 경우
						my_way->key[DEGREE - 1] = x->key[i];
						for (int j = 0; j < right_c->n; j++) {
							my_way->key[j + DEGREE] = right_c->key[j];
						}
						for (int j = 0; j < right_c->n+1; j++) {
							my_way->child[j + DEGREE] = right_c->child[j];
						}
						my_way->n++;
						//check
						for (int j = i; j < x->n - 1; j++) {
							x->key[j] = x->key[j + 1];
							x->child[j + 1] = x->child[j + 1 + 1];
						}
						x->n--;
						free(right_c);
					}
				}
			}
			B_Plus_Tree_Delete_main(my_way, k);
		}
	}
}
