#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>

unsigned long long convert_to_decimal(const char *value, int base) {
    unsigned long long decimal_value = 0;
    int len = strlen(value);
    for (int i = 0; i < len; i++) {
        char digit = value[i];
        int digit_value = (digit >= '0' && digit <= '9') ? digit - '0' : (digit - 'a' + 10);
        decimal_value += digit_value * pow(base, len - i - 1);
    }
    return decimal_value;
}

unsigned long long lagrange_interpolation(int shares[][2], int n, int x_value) {
    double total = 0.0;
    for (int i = 0; i < n; i++) {
        double term = shares[i][1];  // yi
        for (int j = 0; j < n; j++) {
            if (i != j) {
                term *= (double)(x_value - shares[j][0]) / (shares[i][0] - shares[j][0]);
            }
        }
        total += term;
    }
    return (unsigned long long)total;
}

unsigned long long reconstruct_secret(int n, int k, int shares[][2]) {
    return lagrange_interpolation(shares, k, 0);
}

int main() {
   
    int shares1[4][2] = {
        {1, convert_to_decimal("4", 10)},  // (1, 4)
        {2, convert_to_decimal("111", 2)}, // (2, 7)
        {3, convert_to_decimal("12", 10)}, // (3, 12)
        {6, convert_to_decimal("213", 4)}  // (6, 39)
    };

    unsigned long long secret1 = reconstruct_secret(4, 3, shares1);
    printf("secret key from test case 1: %llu\n", secret1);

   
    int shares2[10][2] = {
        {1, convert_to_decimal("420020006424065463", 7)},
        {2, convert_to_decimal("10511630252064643035", 7)},
        {3, convert_to_decimal("101010101001100101011100000001000111010010111101100100010", 2)},
        {4, convert_to_decimal("31261003022226126015", 8)},
        {5, convert_to_decimal("2564201006101516132035", 7)},
        {6, convert_to_decimal("a3c97ed550c69484", 15)},
        {7, convert_to_decimal("134b08c8739552a734", 13)},
        {8, convert_to_decimal("23600283241050447333", 10)},
        {9, convert_to_decimal("375870320616068547135", 9)},
        {10, convert_to_decimal("30140555423010311322515333", 6)}
    };

    unsigned long long secret2 = reconstruct_secret(10, 7, shares2);
    printf(" secret key from test case 2: %llu\n", secret2);

    return 0;
}
