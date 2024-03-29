---
tags:
  - Ad-hoc
---
# Quantum Computer
<span class="author">
Author: cfalas
</span>

## Problem
[Source](http://81.4.170.42:8980/training/#/task/quantum/statement)

Το πρόβλημα μας ζητάει να δημιουργήσουμε μια σειρά από bitwise operators για να μπορέσουμε να αναγνωρίσουμε και να διορθώσουμε μέχρι 2 bit-flips.

## Solution
### Subtasks 1 & 2 (70 points)
- Το πολύ 1 bit-flip

Θα δούμε 4 διαφορετικές λύσεις, με διαφορετικό αριθμό queries:

#### Solution 1 ($K = 2N$)

Μπορούμε να δημιουργήσουμε 2 αντίτυπα των δεδομένων (άρα συνολικά θα έχουμε 3 φορές το κάθε bit). Αφού θα υπάρχει το πολύ 1 bit-flip, τότε ξέρουμε ότι τουλάχιστο τα 2 αντίτυπα του κάθε bit θα είναι σωστά, άρα σε περιπτώσεις που θα 3 αντίτυπα δεν είναι τα ίδια, μπορούμε να επιλέξουμε την τιμή που εμφανίζεται 2 φορές.

!!! example "Παράδειγμα"
    Για παράδειγμα, πιο κάτω:

    $$
    \begin{split}
    011011010 \\ 
    \hline 
    011{\color{red}1}11010 \\
    011011010
    \end{split}
    $$

    Η πρώτη γραμμή αντιπροσωπεύει τα αρχικά δεδομένα, και οι επόμενες 2 τα επιπρόσθετα. Το μόνο λάθος είναι στην δεύτερη γραμμή, το οποίο όμως μπορούμε να ανιχνεύσουμε και να διορθώσουμε, αφού οι άλλες 2 τιμές στην στήλη είναι σωστές.

#### Solution 2 ($K = N + 1$)

Θα προσθέσουμε $N+1$ bits. Το πρώτο θα είναι το XOR όλων των bits. Με τα υπόλοιπα $N$ bits, θα αντιγράψουμε όλα τα προηγούμενα bits όπως στην προηγούμενη λύση, αλλά μόνο μια φορά.

Χρησιμοποιώντας το XOR μπορούμε να δούμε αν υπάρχει λάθος στο πρώτο αντίτυπο, για να ξέρουμε αν πρέπει να χρησιμοποιήσουμε το πρώτο ή το δεύτερο αντίτυπο. Για να πείσουμε τον εαυτό μας ότι αυτό είναι σωστό, θα το χωρίσουμε σε 3 περιπτώσεις:

1. Το λάθος είναι στα αρχικά δεδομένα: Σε αυτή την περίπτωση, το XOR δεν θα ταιριάζει, έτσι θα χρησιμοποιήσουμε το δεύτερο αντίτυπο, το οποίο είναι σωστό.
2. Το λάθος είναι στο bit με το XOR: Όπως και πριν, το XOR δεν θα ταιριάζει, έτσι θα χρησιμοποιήσουμε το δεύτερο αντίτυπο, το οποίο είναι σωστό.
3. Το λάθος είναι στο αντίτυπο των bits: Σε αυτή την περίπτωση, το XOR των αρχικών δεδομένων θα ταιριάζει, και έτσι θα χρησιμοποιήσουμε το πρώτο αντίτυπο.
4. Δεν υπάρχει λάθος: Σε αυτή την περίπτωση, το XOR των αρχικών δεδομένων θα ταιριάζει, και έτσι θα χρησιμοποιήσουμε το πρώτο αντίτυπο.

!!! example "Παράδειγμα"
    $$
    \begin{align}
    S &=  &10111 \\
    D &= 0&10111 \\ \\
    S' &=  &10{\color{red}1}11 \\
    D' &= 0&10111
    \end{align}
    $$

    Σε αυτή την περίπτωση, το XOR των δεδομένων που έχουμε είναι 1, αλλά το XOR που λαμβάνουμε είναι 0, άρα ξέρουμε ότι το λάθος είναι στα αρχικά δεδομένα (ή στο XOR που λάβαμε). Έτσι, ξέρουμε ότι το δεύτερο αντίτυπο είναι σωστό, άρα το χρησιμοποιούμε αυτό.

#### Solution 3 ($K = 2\sqrt{N}$)

Η ιδέα για αυτή τη λύση είναι να γράψουμε τα αρχικά μας δεδομένα σε ένα πίνακα, και να ανιχνεύσουμε σε ποια γραμμή και ποια στήλη είναι το λάθος. Αφού υπάρχει το πολύ 1 λάθος, τότε το parity (δηλαδή το XOR) της κάθε γραμμής θα αλλάξει αν και μόνο αν υπάρχει λάθος. 

!!! example "Παράδειγμα"
    Δηλαδή, θα έχουμε το εξής:

    $$
    \begin{array}{ccc|c}
    0 & 1 & 1 & {\color{blue}0} \\ 
    1 & 1 & 1 & {\color{blue}1} \\ 
    0 & 1 & 0 & {\color{blue}1} \\ 
    \hline 
    {\color{blue} 0} & {\color{blue}1} & {\color{blue}0} \\
    \end{array}
    $$

    Οι τιμές με μπλε αντιπροσωπεύουν τα επιπρόσθετα bits, και υπολογίζονται ως το XOR της γραμμής ή της στήλης. Με αυτό τον τρόπο, μπορούμε να βρούμε σε ποιά στήλη είναι το λάθος. 

!!! note "Σημείωση"
    Στην περίπτωση που το λάθος είναι στα επιπρόσθετα δεδομένα, θα βρούμε στήλη με λάθος, αλλά όχι γραμμή, ή αντίθετα. Αν συμβεί αυτό, ξέρουμε ότι δεν πρέπει να αλλάξουμε κάτι στα δεδομένα.

#### Solution 4 ($K = 1 + \log_2 N$)

Σε αυτή την λύση, στην περίπτωση που υπάρχει λάθος, θα βρούμε την θέση στην οποία είναι το λάθος όπως και στην προηγούμενη λύση, αλλά χρησιμοποιώντας λιγότερα bits.

Συγκεκριμένα, θα χρησιμοποιήσουμε $\log_2 N$ queries τύπου XOR, στα οποία θα ελέγχουμε αν η θέση που περιέχει το λάθος είναι μέσα σε κάποιο σύνολο. Τα σύνολα που θα επιλέξουμε είναι όλες οι δυνάμεις του δύο. Δηλαδή, στο πρώτο additional bit θα βάλουμε το XOR όλων των bits που έχουν 0 στην πρώτη θέση (δηλαδή είναι ζυγά), στο δεύτερο bit θα βάλουμε το XOR όλων των bits που έχουν 0 στη δεύτερη θέση (δηλαδή που έχουν υπόλοιπο 0 ή 1 όταν διαιρεθούν με το 4), κλπ.

Με αυτό τον τρόπο, όταν ελέγχουμε για λάθος, ξέρουμε με κάθε έλεγχο ένα επιπρόσθετο bit για την θέση στην οποία έγινε το λάθος. Όμως, για να είμαστε σίγουροι ότι έγινε κάποιο λάθος, μπορούμε να προσθέσουμε επίσης ένα bit με το XOR όλων των bits όπως στην [δεύτερη λύση](#solution-2-k-n-1)

!!! example "Παράδειγμα"

    $$
    \begin{align}
    S &= 01011010 \\
    b_0 &= S_0\oplus S_1 \oplus S_2 \oplus S_3 \oplus S_4 \oplus S_5 \oplus S_6 \oplus S_7 &= 0\\
    b_1 &= S_0 \oplus S_2 \oplus S_4 \oplus S_6 &=0\\
    b_2 &= S_0 \oplus S_4  &=1\\
    b_3 &= S_0 &=0\\ \\
    S' &= 0101{\color{red}0}010 \\
    b'_0 &= S'_0\oplus S'_1 \oplus S'_2 \oplus S'_3 \oplus S'_4 \oplus S'_5 \oplus S'_6 \oplus S'_7 &\neq b_0 \\
    b'_1 &= S'_0 \oplus S'_2 \oplus S'_4 \oplus S'_6  &\neq b_1\\
    b'_2 &= S'_0 \oplus S'_1 \oplus S'_4 \oplus S'_5  &\neq b_2\\
    b'_3 &= S'_0 \oplus S'_1 \oplus S'_2 \oplus S'_3 & = b_3\\
    \end{align}
    $$

    Από αυτά, μπορούμε να δούμε:

    $$
    \begin{align}
    b_1 \neq b'_1 \implies k_0 = 0 \\
    b_2 \neq b'_2 \implies k_1 = 0 \\
    b_3 = b'_3 \implies k_2 = 1 \\
    \therefore k = 100_2 = 4
    \end{align} 
    $$
    

### Subtask 3 (30 points)

Σε αυτό το subtask, μπορούν να υπάρχουν μέχρι και 2 λάθη αντί μόνο ένα λάθος. Θα δούμε 3 λύσεις, οι οποίες κάνουν extend τις αντίστοιχες λύσεις του 2ου subtask.
#### Solution 1 ($K = 4N$)
Όπως και στο subtask 2, θα δημιουργούμε αντίτυπα, έτσι ώστε αν υπάρχει κάποιο λάθος, η πλειοψηφία των bits να παραμείνει σωστή. Αφού τα 2 bits μπορεί να βρίσκονται σε θέσεις που αντιστοιχούν στο ίδιο bit, χρειαζόμαστε τουλάχιστο 5 αντίτυπα για να έχουμε πλειοψηφία από σωστά με 2 λάθη.

#### Solution 3 ($K = N + 2\sqrt{N} + 2$)
Θα χρησιμοποιήσουμε την ιδέα με το grid, αλλά για να διορθώσουμε 2 λάθη θα χρειαστούμε 2 αντίτυπα ολόκληρου του grid (δηλαδή 1 αντίτυπο των δεδομένων, και 2 φορές τα parities).

Θα κάνουμε την εξής αλλαγή: Για το κάθε grid, θα προσθέτουμε και το τελευταίο κελί το οποίο αφήσαμε κενό στο subtask 2 (χρησιμοποιώντας το XOR της τελευταίας γραμμής ή της στήλης). Με αυτό τον τρόπο, θα μπορούμε να ανιχνεύσουμε αν υπάρχουν 2 λάθη στο grid, αφού υπάρχουν οι ακόλουθες περιπτώσεις:

- Αν υπάρχει 1 λάθος στα δεδομένα, θα υπάρχουν 2 λάθος XOR (της γραμμής και της στήλης)
- Αν υπάρχει 1 λάθος στα XOR κάποιας γραμμής/στήλης, θα υπάρχουν 2 λάθος XOR (της συγκεκριμένης γραμμής και της στήλης)
- Αν υπάρχει 1 λάθος στο τελευταίο κελί, θα υπάρχει ένα λάθος XOR, στο τελευταίο κελί του grid, αλλά θα είναι λάθος και στις 2 κατευθύνσεις (ως προς τις γραμμές και τις στήλες).
- Αν υπάρχουν 2 λάθη στα δεδομένα, θα υπάρχουν 4 λάθος XOR (2 γραμμές και 2 στήλες, αλλά δε θα μπορούμε να καταλάβουμε ακριβώς που είναι το λάθος)
- Αν υπάρχει 1 λάθος στα δεδομένα και 1 λάθος σε διαφορετική γραμμή/στήλη, θα υπάρχουν 4 λάθος XOR (η γραμμή και η στήλη που περιέχουν τα λάθος δεδομένα, η γραμμή/στήλη που περιέχει το λάθος XOR, και το τελευταίο κελί ως προς τη μια κατεύθυνση)
- Αν υπάρχει 1 λάθος στα δεδομένα και 1 λάθος στην ίδια γραμμή/στήλη, θα υπάρχουν 2 λάθος XOR (η γραμμή/στήλη που περιέχει τα λάθος δεδομένα και το τελευταίο κελί ως προς τη μια κατεύθυνση)
- Αν υπάρχει 1 λάθος στα δεδομένα και 1 λάθος στο τελευταίο κελί, θα υπάρχουν 4 λάθος XOR (η γραμμή και η στήλη που περιέχουν τα λάθος δεδομένα, και το τελευταίο κελί ως προς και τις 2 κατευθύνσεις)
- ...

Αν αναλύσουμε όλες τις πιθανές περιπτώσεις, μπορούμε να καταλήξουμε στην ακόλουθη στρατηγική:

1. Μετρούμε τον αριθμό των γραμμών που έχουν λάθος XOR (συμπεριλαμβανομένων και της τελευταίας)
2. Μετρούμε τον αριθμό των στηλών που έχουν λάθος XOR (συμπεριλαμβανομένων και της τελευταίας)
3. Ο αριθμός των λαθών στο grid είναι το μέγιστο πλήθος γραμμών/στηλών που είναι λάθος

Υπάρχουν οι εξής περιπτώσεις:

1. Το πρώτο grid δεν έχει λάθη: Άρα μπορούμε να επιστρέψουμε τα δεδομένα
2. Το πρώτο grid έχει 1 λάθος: Μπορούμε να χρησιμοποιήσουμε το grid όπως στο subtask 2
3. Το πρώτο grid έχει 2 λάθη: Μπορούμε να αγνοήσουμε το πρώτο grid, και να χρησιμοποιήσουμε το 2ο grid, το οποίο δεν περιέχει λάθη (αφού συνολικά υπάρχουν μέχρι 2 λάθη)


#### Solution 4 ($K = N + 2\log{N} + 4$)
Μπορούμε να χρησιμοποιήσουμε την ίδια ιδέα του "ανιχνεύω αν υπάρχουν 1 ή 2 λάθη στο πρώτο αντίτυπο", και χρησιμοποιώ το αντίτυπο το οποίο έχει μέχρι ένα λάθος.

Για να μπορέσουμε να ανιχνεύσουμε αν υπάρχουν 1 ή 2 λάθη θα χρειαστούν κάποιες αλλαγές από την αντίστοιχη λύση του subtask 2. Θα χρειαστούμε ένα επιπρόσθετο bit, το οποίο θα μας επιτρέπει να ανιχνεύουμε αν υπήρχαν 2 λάθη. Το καινούργιο bit θα είναι το XOR όλων των προηγούμενων. Έτσι, θα έχουμε τις περιπτώσεις:

1. Το τελευταίο XOR είναι σωστό - Υπήρχαν 0 ή 2 λάθη. Αν υπήρχαν 0 λάθη, τότε και το πρώτο XOR θα είναι σωστό
2. Το τελευταίο XOR είναι λάθος - Υπήρχε ακριβώς 1 λάθος. Μπορούμε να χρησιμοποιήσουνε αυτό το αντίτυπο.

Η τεχνική που χρησιμοποιήσαμε ονομάζεται [Hamming code](https://en.wikipedia.org/wiki/Hamming_code), και μπορεί να γίνει extend
