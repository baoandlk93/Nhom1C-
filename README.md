# Nhom1C-

Nhom1 môn C++

---

## 1.1. Sum from 1 to N

### Đề bài

Cho số nguyên dương $N$. Tính tổng $S = 1 + 2 + \dots + N$.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($T \le 10$). $T$ dòng tiếp theo, mỗi dòng chứa một số nguyên dương $N$ ($N \le 10^9$).
- **Output:** Với mỗi bộ test, in kết quả trên một dòng.

### Ý tưởng

- Sử dụng công thức cấp số cộng $S = \frac{N \times (N + 1)}{2}$ với độ phức tạp `O(1)` để tránh quá thời gian (TLE).
- Sử dụng kiểu dữ liệu `long long` vì khi $N = 10^9$, kết quả $\approx 5 \times 10^{17}$ vượt quá giới hạn kiểu `int`.

### Code C++

```cpp
#include <iostream>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        long long N;
        cin >> N;

        long long S = N * (N + 1) / 2;
        cout << S << "\n";
    }

    return 0;
}
```

---

## 1.11. Sphenic Number

### Đề bài

Số nguyên dương $N$ được gọi là số Sphenic nếu $N$ được phân tích duy nhất dưới dạng tích của đúng 3 thừa số nguyên tố khác nhau (ví dụ: $30 = 2 \times 3 \times 5$ là số Sphenic; $60 = 2^2 \times 3 \times 5$ không phải).

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo chứa một số nguyên dương $N$ ($1 \le N \le 10000$).
- **Output:** Với mỗi bộ test, in ra `1` nếu $N$ là số Sphenic, ngược lại in ra `0`.

### Ý tưởng

- Phân tích $N$ thành thừa số nguyên tố bằng cách duyệt từ $2$ đến $\sqrt{N}$.
- Đếm số lượng thừa số nguyên tố phân biệt (`cnt`) và số mũ của từng thừa số (`exp`). Nếu bất kỳ thừa số nào có `exp > 1`, trả về `false`.
- Sau vòng lặp, nếu $N > 1$ thì tăng `cnt` thêm 1. Kiểm tra `cnt == 3`.

### Code C++

```cpp
#include <iostream>

using namespace std;

bool isSphenic(int n) {
    int cnt = 0;

    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            int exp = 0;
            while (n % i == 0) {
                exp++;
                n /= i;
            }
            if (exp > 1) return false;
            cnt++;
        }
    }

    if (n > 1) {
        cnt++;
    }

    return cnt == 3;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        int N;
        cin >> N;
        cout << (isSphenic(N) ? 1 : 0) << "\n";
    }

    return 0;
}
```

---

## 1.12. Divisors Divisible by 2

### Đề bài

Cho số nguyên dương $N$. Hãy đếm xem $N$ có bao nhiêu ước số chia hết cho 2.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($T \le 100$). Mỗi bộ test gồm một số nguyên $N$ ($1 \le N \le 10^9$).
- **Output:** Với mỗi bộ test, in ra số lượng ước chia hết cho 2 của $N$ trên một dòng.

### Ý tưởng

- Nếu $N$ là số lẻ (`N % 2 != 0`), chắc chắn $N$ không có ước nào chia hết cho 2 $\rightarrow$ kết quả là `0`.
- Duyệt `i` từ $1$ đến $\sqrt{N}$ (`i * i <= N`) để tìm các cặp ước số `i` và `N / i` trong thời gian `O(√N)`:
  - Nếu `N % i == 0`:
    - Kiểm tra nếu `i % 2 == 0` thì tăng biến đếm.
    - Kiểm tra ước đối xứng `N / i`: nếu `N / i != i` (tránh đếm trùng khi $N$ là số chính phương) và `(N / i) % 2 == 0` thì tăng biến đếm.

### Code C++

```cpp
#include <iostream>

using namespace std;

int countEvenDivisors(long long n) {
    if (n % 2 != 0) return 0;

    int cnt = 0;
    for (long long i = 1; i * i <= n; i++) {
        if (n % i == 0) {
            if (i % 2 == 0) {
                cnt++;
            }
            if (i != n / i && (n / i) % 2 == 0) {
                cnt++;
            }
        }
    }
    return cnt;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        long long N;
        cin >> N;
        cout << countEvenDivisors(N) << "\n";
    }

    return 0;
}
```

---

## 1.13. Divisible by A and B

### Đề bài

Cho 4 số nguyên dương $M, N, A, B$. Đếm tất cả các số nguyên trong đoạn $[M, N]$ chia hết cho $A$ hoặc chia hết cho $B$.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo gồm 4 số $M, N, A, B$ ($1 \le M, N \le 10^5; 1 \le A, B \le 500$).
- **Output:** Với mỗi bộ test, in ra kết quả trên một dòng.

### Ý tưởng

- **Cách 1 (Duyệt trực tiếp - `O(N - M)`):** Vì $M, N \le 10^5$ khá nhỏ, ta có thể duyệt `i` từ $M$ đến $N$ (đảm bảo $M \le N$ bằng `swap` nếu cần) và kiểm tra điều kiện `i % A == 0 || i % B == 0`.
- **Cách 2 (Nguyên lý bù trừ - `O(log(min(A, B)))`):** Số lượng các số trong $[M, N]$ chia hết cho $X$ là $\lfloor \frac{N}{X} \rfloor - \lfloor \frac{M - 1}{X} \rfloor$. Áp dụng nguyên lý bù trừ:
  $$\text{Kết quả} = \text{count}(A) + \text{count}(B) - \text{count}(\text{LCM}(A, B))$$

### Code C++

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        int M, N, A, B;
        cin >> M >> N >> A >> B;

        if (M > N) swap(M, N);

        int cnt = 0;
        for (int i = M; i <= N; i++) {
            if (i % A == 0 || i % B == 0) {
                cnt++;
            }
        }

        cout << cnt << "\n";
    }

    return 0;
}
```

---

## 1.15. First Prime Pair Sum to N

### Đề bài

Cho số tự nhiên $N$. Hãy tìm cặp số nguyên tố đầu tiên có tổng bằng $N$. Nếu không tồn tại cặp số nguyên tố nào có tổng bằng $N$, in ra `-1`.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo chứa một số tự nhiên $N$ ($1 \le N \le 10^6$).
- **Output:** Với mỗi bộ test, in ra cặp số nguyên tố đầu tiên (cách nhau bởi dấu cách) hoặc `-1` trên một dòng.

### Ý tưởng

- Vì $N \le 10^6$ và có nhiều bộ test $T \le 100$, ta sử dụng **Sàng số nguyên tố Eratosthenes** để đánh dấu trước tất cả các số nguyên tố $\le 10^6$ trong thời gian `O(MAXN log log MAXN)`.
- Với mỗi giá trị $N$, duyệt $p$ từ $2$ đến $N / 2$:
  - Nếu cả $p$ và $N - p$ đều là số nguyên tố thì in ra `p` và `N - p`, sau đó dừng vòng lặp ngay để đảm bảo đó là cặp đầu tiên.
- Nếu duyệt hết mà không tìm thấy cặp nào, in ra `-1`.

### Code C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

const int MAXN = 1000000;
vector<bool> isPrime(MAXN + 1, true);

void sieve() {
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= MAXN; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= MAXN; j += i) {
                isPrime[j] = false;
            }
        }
    }
}

void solve(int n) {
    for (int p = 2; p <= n / 2; p++) {
        if (isPrime[p] && isPrime[n - p]) {
            cout << p << " " << n - p << "\n";
            return;
        }
    }
    cout << -1 << "\n";
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    sieve();

    int T;
    cin >> T;

    while (T--) {
        int N;
        cin >> N;
        solve(N);
    }

    return 0;
}
```

---

## 1.16. Factorial Divisors

### Đề bài

Cho số tự nhiên $N$ và số nguyên tố $P$. Hãy tìm số nguyên lớn nhất $x$ sao cho $N!$ chia hết cho $P^x$.
Ví dụ: với $N = 7, P = 3$ thì $x = 2$ là giá trị lớn nhất để $7!$ chia hết cho $3^2$.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo chứa cặp số $N$ và $P$ cách nhau bởi dấu cách ($1 \le N \le 10^5; 2 \le P \le 5000$).
- **Output:** Với mỗi bộ test, in ra kết quả trên một dòng.

### Ý tưởng

- Sử dụng **Công thức Legendre** để tính số mũ lớn nhất của số nguyên tố $P$ trong phân tích thừa số nguyên tố của $N!$:
  $$x = \left\lfloor \frac{N}{P} \right\rfloor + \left\lfloor \frac{N}{P^2} \right\rfloor + \left\lfloor \frac{N}{P^3} \right\rfloor + \dots$$
- Thay vì tính $P^k$ (có thể gây tràn số), ta liên tục lấy `N /= P` và cộng dồn `N` vào `x` cho đến khi `N == 0`. Độ phức tạp chỉ là `O(log_P(N))`.

### Code C++

```cpp
#include <iostream>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        int N, P;
        cin >> N >> P;

        int x = 0;
        while (N > 0) {
            N /= P;
            x += N;
        }

        cout << x << "\n";
    }

    return 0;
}
```

---

## 1.18. Numbers with Three Divisors

### Đề bài

Cho số tự nhiên $N$. Hãy liệt kê tất cả các số $\le N$ có đúng 3 ước số.
Ví dụ: với $N = 100$, các số thỏa mãn là `4 9 25 49`.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo chứa một số tự nhiên $N$ ($1 \le N \le 10^6$).
- **Output:** Với mỗi bộ test, in ra các số thỏa mãn (cách nhau bởi dấu cách) trên một dòng.

### Ý tưởng

- Một số nguyên dương có **đúng 3 ước số** khi và chỉ khi nó là **bình phương của một số nguyên tố** ($p^2$ với $p$ là số nguyên tố, khi đó 3 ước số là $1, p, p^2$).
- Vì $N \le 10^6 \Rightarrow p \le \sqrt{10^6} = 1000$.
- Ta dùng **Sàng Eratosthenes** để tìm tất cả các số nguyên tố $\le 1000$. Với mỗi test case $N$, duyệt các số nguyên tố $p$ sao cho $p^2 \le N$ và in ra $p^2$.

### Code C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

const int MAXP = 1000;
vector<bool> isPrime(MAXP + 1, true);
vector<int> primes;

void sieve() {
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= MAXP; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= MAXP; j += i) {
                isPrime[j] = false;
            }
        }
    }
    for (int i = 2; i <= MAXP; i++) {
        if (isPrime[i]) {
            primes.push_back(i);
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    sieve();

    int T;
    cin >> T;

    while (T--) {
        long long N;
        cin >> N;

        for (int p : primes) {
            if (1LL * p * p <= N) {
                cout << 1LL * p * p << " ";
            } else {
                break;
            }
        }
        cout << "\n";
    }

    return 0;
}
```

---

## 1.19. Smith Number

### Đề bài

Cho số tự nhiên $N$. Kiểm tra xem $N$ có phải là số **Smith** hay không.
Một số được gọi là số Smith nếu:

1. $N$ **không phải** là số nguyên tố (tức $N$ là hợp số).
2. Tổng các chữ số của $N$ bằng tổng các chữ số của tất cả các thừa số nguyên tố của $N$ (tính cả lặp lại).
   Ví dụ: $N = 666 = 2 \times 3 \times 3 \times 37$ có tổng chữ số các ước nguyên tố là $2 + 3 + 3 + (3 + 7) = 18$, bằng tổng chữ số của $666$ ($6 + 6 + 6 = 18$).

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo chứa một số nguyên $N$ ($1 \le N \le 100000$).
- **Output:** Với mỗi bộ test, in ra `YES` nếu $N$ là số Smith, ngược lại in ra `NO`.

### Ý tưởng

- Viết hàm `sumDigits(int x)` để tính tổng các chữ số của một số.
- Phân tích $N$ thành các thừa số nguyên tố bằng cách duyệt $i$ từ $2$ đến $\sqrt{N}$:
  - Mỗi lần $N$ chia hết cho $i$, cộng `sumDigits(i)` vào tổng `sumFactors` và gán `N /= i`.
- Lưu ý điều kiện **$N$ không phải số nguyên tố**: nếu ban đầu $N \le 3$ hoặc sau vòng lặp từ $2$ đến $\sqrt{N}$ mà $N$ vẫn giữ nguyên giá trị ban đầu (`N == originalN`) thì $N$ không phải hợp số $\rightarrow$ trả về `false` (`NO`).
- Nếu sau vòng lặp phần dư $N > 1$, cộng thêm `sumDigits(N)` vào `sumFactors`.
- So sánh `sumDigits(originalN) == sumFactors`.

### Code C++

```cpp
#include <iostream>

using namespace std;

int sumDigits(int n) {
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

bool isSmith(int n) {
    if (n <= 3) return false;

    int originalN = n;
    int sumFactors = 0;

    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            int d = sumDigits(i);
            while (n % i == 0) {
                sumFactors += d;
                n /= i;
            }
        }
    }

    if (n == originalN) return false;

    if (n > 1) {
        sumFactors += sumDigits(n);
    }

    return sumDigits(originalN) == sumFactors;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        int N;
        cin >> N;
        if (isSmith(N)) {
            cout << "YES\n";
        } else {
            cout << "NO\n";
        }
    }

    return 0;
}
```

---

## 1.20. Remainder Sum from 1 to N

### Đề bài

Cho hai số nguyên không âm $N$ và $K$. Tính tổng:
$$S = (1 \bmod K) + (2 \bmod K) + \dots + (N \bmod K)$$
Thuật toán cần đạt độ phức tạp hằng số `O(1)`.

- **Input:** Dòng đầu tiên là số bộ test $T$ ($1 \le T \le 100$). Mỗi dòng tiếp theo chứa cặp số $N$ và $K$ cách nhau bởi dấu cách ($0 \le N \le 10^{12}; 0 \le K \le 10^{12}$).
- **Output:** Với mỗi bộ test, in ra kết quả trên một dòng.

### Ý tưởng

- Khi chia các số liên tiếp $1, 2, \dots, N$ cho $K$, số dư sẽ lặp lại theo chu kỳ có độ dài $K$:
  $$1, 2, \dots, K - 1, 0$$
- Số chu kỳ đầy đủ từ $1$ đến $N$ là $q = \lfloor N / K \rfloor$. Tổng một chu kỳ đầy đủ là:
  $$S_{\text{chu kỳ}} = 1 + 2 + \dots + (K - 1) = \frac{(K - 1) \times K}{2}$$
- Phần dư còn lại gồm $r = N \bmod K$ số cuối cùng có số dư lần lượt là $1, 2, \dots, r$. Tổng phần này là:
  $$S_{\text{dư}} = 1 + 2 + \dots + r = \frac{r \times (r + 1)}{2}$$
- Vậy công thức `O(1)` là:
  $$S = q \times \frac{(K - 1) \times K}{2} + \frac{r \times (r + 1)}{2}$$

### Code C++

```cpp
#include <iostream>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;

    while (T--) {
        long long N, K;
        cin >> N >> K;

        if (K == 0 || N == 0) {
            cout << 0 << "\n";
            continue;
        }

        long long q = N / K;
        long long r = N % K;

        long long sumCycle = (K - 1) * K / 2;
        long long sumRem = r * (r + 1) / 2;

        long long S = q * sumCycle + sumRem;
        cout << S << "\n";
    }

    return 0;
}
```
