## Template (With queries x and idx)
```cpp
//"mobius : number of pairs gcd(a[i],a[j]) = x, given x and i in a query and update queries"

/* Includes */
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
/*________________________________________________*/
/* using namespace */

using namespace std;
using namespace __gnu_pbds;
/*________________________________________________*/

/* Defines*/
#define ll long long
#define ACCEPTED 0
#define fastIO ios_base::sync_with_stdio(false), cin.tie(NULL), cout.tie(NULL);
#define all(x) x.begin(), x.end()
/*________________________________________________*/
const ll N = 1e6;
/*________________________________________________*/
vector<ll> divs[N + 5];
ll mu[N + 5];
ll frq_divisible[N + 5];
void compute_mobius()
{
    mu[1] = 1;
    for (ll i = 1; i <= N; i++)
    {
        for (ll j = 2 * i; j <= N; j += i)
        {
            mu[j] -= mu[i];
        }
    }
}
void pre_compute_divisors()
{
    for (ll i = 1; i <= N; i++)
    {
        for (ll j = i; j <= N; j += i)
        {
            divs[j].push_back(i);
        }
    }
}
void tc()
{
    ll n, q;
    cin >> n >> q;

    vector<ll> vec(n);
    for (ll i = 0; i < n; i++)
    {
        cin >> vec[i];
        for (auto d : divs[vec[i]])
        {
            frq_divisible[d]++;
        }
    }

    while (q--)
    {
        ll op, idx;
        cin >> op >> idx;
        idx--;
        if (op == 1)
        {
            ll g;
            cin >> g;
            if (vec[idx] % g != 0)
            {
                cout << "0\n";
                continue;
            }

            ll num = vec[idx] / g;
            ll ans = (vec[idx] == g) ? -1 : 0;
            for (auto d : divs[num])
            {
                ans += mu[d] * frq_divisible[d * g];
            }
            cout << ans << "\n";
        }
        else
        {
            ll x;
            cin >> x;
            for (auto d : divs[vec[idx]])
            {
                frq_divisible[d]--;
            }
            vec[idx] = x;
            for (auto d : divs[vec[idx]])
            {
                frq_divisible[d]++;
            }
        }
    }

    for (ll i = 0; i < n; i++)
    {
        for (auto d : divs[vec[i]])
        {
            frq_divisible[d]--;
        }
    }
}

signed main()
{
    fastIO;
    compute_mobius();
    pre_compute_divisors();

    ll t;
    cin >> t;
    while (t--)
    {
        tc();
    }
    return ACCEPTED;
}
```
## Template (No Queries)
```cpp
//"mobius : number of pairs gcd(a[i],a[j]) = x, given x only at the beginning (no queries)"


/* Includes */
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
/*________________________________________________*/
/* using namespace */

using namespace std;
using namespace __gnu_pbds;
/*________________________________________________*/

/* Defines*/
#define ll long long
#define ACCEPTED 0
#define fastIO ios_base::sync_with_stdio(false), cin.tie(NULL), cout.tie(NULL);
#define all(x) x.begin(), x.end()
/*________________________________________________*/

/* Constants */

const ll N = 1e6;

/*________________________________________________*/
ll mu[N + 5];
ll cnt[N + 5];
ll frq_divisible[N + 5];
void compute_mobius()
{
    mu[1] = 1;
    for (ll i = 1; i <= N; i++)
    {
        for (ll j = 2 * i; j <= N; j += i)
        {
            mu[j] -= mu[i];
        }
    }
}

void tc()
{
    ll n, g = 1;
    cin >> n;

    vector<ll> vec(n);
    for (ll i = 0; i < n; i++)
    {
        cin >> vec[i];
        cnt[vec[i]]++;
    }

    for (int d = 1; d <= N; d++)
    {
        for (int m = d; m <= N; m += d)
        {
            frq_divisible[d] += cnt[m];
        }
    }

    ll ans = 0;
    for (ll k = 1; k * g <= N; k++)
    {
        if (mu[k] == 0)
            continue;
        ll mult_count = frq_divisible[k * g];
        ans += mu[k] * (mult_count * (mult_count - 1) / 2);
    }
    cout << (ans) << "\n";
}
signed main()
{
    fastIO;
    compute_mobius();

    ll t = 1;
    // cin >> t;
    while (t--)
    {
        tc();
    }
    return ACCEPTED;
}
```