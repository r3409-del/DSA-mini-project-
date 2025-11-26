#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX_NAME_LEN 100
#define INITIAL_CAPACITY 100

typedef struct {
    char original[MAX_NAME_LEN];
    char cleaned[MAX_NAME_LEN];
    int number;
} NameRecord;

int letterValues[26];
int freq[10];
NameRecord *records = NULL;
int recordCount = 0;
int recordCapacity = 0;

void initLetterValues();
void initRecords();
void addRecord(const char *originalName, const char *cleanedName, int number);
void cleanName(const char *src, char *dest);
int computeNumerologyNumber(const char *name);
int reduceToSingleDigit(int num);
void displayAllRecords();
void displayFrequencyTable();
void displayMostCommonNumbers();
void displayPatternObservations();

int main(void) {
    char input[MAX_NAME_LEN];

    initLetterValues();
    initRecords();

    for (int i = 0; i < 10; i++)
        freq[i] = 0;

    printf("=== DSA Numerology Analyzer (C Version) ===\n");
    printf("Enter names to compute numerology numbers.\n");
    printf("Type 'done' to finish and see analysis.\n\n");

    while (1) {
        printf("Enter a name (or 'done'): ");

        if (!fgets(input, sizeof(input), stdin))
            break;

        size_t len = strlen(input);
        if (len > 0 && input[len - 1] == '\n')
            input[len - 1] = '\0';

        if (strcasecmp(input, "done") == 0)
            break;

        if (strlen(input) == 0) {
            printf("Name cannot be empty. Try again.\n\n");
            continue;
        }

        char cleaned[MAX_NAME_LEN];
        cleanName(input, cleaned);

        int number = computeNumerologyNumber(cleaned);

        addRecord(input, cleaned, number);

        if (number >= 1 && number <= 9)
            freq[number]++;

        printf("Numerology number for \"%s\" is: %d\n\n", input, number);
    }

    if (recordCount == 0) {
        printf("\nNo names were entered. Exiting.\n");
    } else {
        printf("\n=== SUMMARY & ANALYSIS ===\n");
        displayAllRecords();
        displayFrequencyTable();
        displayMostCommonNumbers();
        displayPatternObservations();
    }

    free(records);
    return 0;
}

void initLetterValues() {
    for (int i = 0; i < 26; i++)
        letterValues[i] = 0;

    #define SET_LETTERS(str, val)                   \
        do {                                        \
            const char *p = (str);                  \
            while (*p) {                            \
                char ch = *p;                       \
                if (ch >= 'A' && ch <= 'Z')         \
                    letterValues[ch - 'A'] = (val); \
                p++;                                \
            }                                       \
        } while (0)

    SET_LETTERS("AJS", 1);
    SET_LETTERS("BKT", 2);
    SET_LETTERS("CLU", 3);
    SET_LETTERS("DMV", 4);
    SET_LETTERS("ENW", 5);
    SET_LETTERS("FOX", 6);
    SET_LETTERS("GPY", 7);
    SET_LETTERS("HQZ", 8);
    SET_LETTERS("IR",  9);

    #undef SET_LETTERS
}

void initRecords() {
    recordCapacity = INITIAL_CAPACITY;
    recordCount = 0;

    records = (NameRecord *)malloc(recordCapacity * sizeof(NameRecord));
    if (records == NULL) {
        fprintf(stderr, "Memory allocation failed.\n");
        exit(EXIT_FAILURE);
    }
}

void addRecord(const char *originalName, const char *cleanedName, int number) {
    if (recordCount >= recordCapacity) {
        recordCapacity *= 2;
        NameRecord *newRecords = (NameRecord *)realloc(records, recordCapacity * sizeof(NameRecord));
        if (newRecords == NULL) {
            fprintf(stderr, "Memory reallocation failed.\n");
            free(records);
            exit(EXIT_FAILURE);
        }
        records = newRecords;
    }

    strncpy(records[recordCount].original, originalName, MAX_NAME_LEN - 1);
    records[recordCount].original[MAX_NAME_LEN - 1] = '\0';

    strncpy(records[recordCount].cleaned, cleanedName, MAX_NAME_LEN - 1);
    records[recordCount].cleaned[MAX_NAME_LEN - 1] = '\0';

    records[recordCount].number = number;

    recordCount++;
}

void cleanName(const char *src, char *dest) {
    int j = 0;
    for (int i = 0; src[i] != '\0'; i++) {
        char ch = src[i];
        if (isalpha((unsigned char)ch)) {
            ch = toupper((unsigned char)ch);
            dest[j++] = ch;
        }
    }
    dest[j] = '\0';
}

int computeNumerologyNumber(const char *cleanedName) {
    int sum = 0;
    for (int i = 0; cleanedName[i] != '\0'; i++) {
        char ch = cleanedName[i];
        if (ch >= 'A' && ch <= 'Z') {
            int value = letterValues[ch - 'A'];
            sum += value;
        }
    }
    return reduceToSingleDigit(sum);
}

int reduceToSingleDigit(int num) {
    if (num <= 0)
        return 0;

    while (num > 9) {
        int digitSum = 0;
        while (num > 0) {
            digitSum += num % 10;
            num /= 10;
        }
        num = digitSum;
    }
    return num;
}

void displayAllRecords() {
    printf("\nAll Name Records:\n");
    for (int i = 0; i < recordCount; i++) {
        printf("- Original: \"%s\" | Cleaned: \"%s\" | Number: %d\n",
               records[i].original,
               records[i].cleaned,
               records[i].number);
    }
}

void displayFrequencyTable() {
    printf("\nFrequency of Numerology Numbers:\n");
    for (int i = 1; i <= 9; i++) {
        printf("Number %d: %d name(s)\n", i, freq[i]);
    }
}

void displayMostCommonNumbers() {
    int maxFreq = 0;

    for (int i = 1; i <= 9; i++)
        if (freq[i] > maxFreq)
            maxFreq = freq[i];

    if (maxFreq == 0) {
        printf("\nNo frequency data available.\n");
        return;
    }

    printf("\nMost Common Numerology Number(s): ");
    for (int i = 1; i <= 9; i++)
        if (freq[i] == maxFreq)
            printf("%d ", i);
    printf("with frequency: %d\n", maxFreq);

    for (int n = 1; n <= 9; n++) {
        if (freq[n] == maxFreq) {
            printf("Names with number %d:\n", n);
            for (int i = 0; i < recordCount; i++)
                if (records[i].number == n)
                    printf("  - %s\n", records[i].original);
        }
    }
}

void displayPatternObservations() {
    printf("\nPattern Observations:\n");

    int missingExist = 0;
    printf("- Numbers that did NOT appear: ");
    for (int i = 1; i <= 9; i++) {
        if (freq[i] == 0) {
            printf("%d ", i);
            missingExist = 1;
        }
    }
    if (!missingExist)
        printf("None (all from 1 to 9 are present)");
    printf("\n");

    int lowCount = 0, midCount = 0, highCount = 0;
    for (int i = 1; i <= 9; i++) {
        if (i >= 1 && i <= 4)
            lowCount += freq[i];
        else if (i == 5)
            midCount += freq[i];
        else
            highCount += freq[i];
    }

    printf("- Total names with numbers 1–4: %d\n", lowCount);
    printf("- Total names with number 5: %d\n", midCount);
    printf("- Total names with numbers 6–9: %d\n", highCount);

    if (lowCount > highCount && lowCount > midCount)
        printf("- Observation: Lower numbers (1–4) are more frequent.\n");
    else if (highCount > lowCount && highCount > midCount)
        printf("- Observation: Higher numbers (6–9) are more frequent.\n");
    else if (midCount > 0 && midCount >= lowCount && midCount >= highCount)
        printf("- Observation: Number 5 occurs very frequently.\n");
    else
        printf("- Observation: Frequencies are fairly balanced.\n");
}
