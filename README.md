#include <stdio.h>

typedef long long ll;

// Calcul du PGCD
ll pgcd(ll a, ll b) {
    while (b != 0) {
        ll t = b;
        b = a % b;
        a = t;
    }
    return a >= 0 ? a : -a;
}

// Calcul du PPCM
ll ppcm(ll a, ll b) {
    ll g = pgcd(a, b);
    return (a / g) * b;
}

// Euclide étendu
ll extended_gcd(ll a, ll b, ll *x, ll *y) {
    if (b == 0) {
        *x = 1;
        *y = 0;
        return a >= 0 ? a : -a;
    }
    ll x1, y1;
    ll d = extended_gcd(b, a % b, &x1, &y1);
    *x = y1;
    *y = x1 - (a / b) * y1;
    return d;
}

int main() {
    ll a, b, c;
    printf("Entrer a b c : ");
    if (scanf("%lld %lld %lld", &a, &b, &c) != 3)
        return 1;

    // Calcul PGCD, PPCM et coprimalité
    ll g = pgcd(a, b);
    ll l = ppcm(a, b);

    printf("\nInformations sur les nombres :\n");
    printf("PGCD(%lld, %lld) = %lld\n", a, b, g);
    printf("PPCM(%lld, %lld) = %lld\n", a, b, l);

    if (g == 1)
        printf("%lld et %lld sont premiers entre eux.\n", a, b);
    else
        printf("%lld et %lld ne sont PAS premiers entre eux.\n", a, b);

    // Résolution de l'équation diophantienne
    ll x0, y0;
    ll d = extended_gcd(a, b, &x0, &y0);

    if (c % d != 0) {
        printf("Pas de solution.\n");
        return 0;
    }

    x0 *= c / d;
    y0 *= c / d;

    ll bx = b / d;
    ll ay = a / d;

    printf("\nSolutions dans x ∈ [1,12], y ∈ [1,31] :\n");
    for (ll k = -10000; k <= 10000; k++) {
        ll x = x0 + bx * k;
        ll y = y0 - ay * k;

        if (x >= 1 && x <= 12 && y >= 1 && y <= 31) {
            printf("  x = %lld, y = %lld\n", x, y);
        }
    }

    return 0;
}
