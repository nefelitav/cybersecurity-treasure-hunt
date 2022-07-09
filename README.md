## 2022 Project 2

![](logo.png)

Ερωτήσεις:

1. Πού βρίσκεται ο Γιώργος;
1. Τι βρήκε ο Γιώργος;
1. Τι ώρα είναι στο "Plan X";
1. Πού βρίσκονται τα αρχεία του "Plan X";
1. Ποια είναι τα results του "Plan Y";
1. Ποιο είναι το code του "Plan Z";

#### Παρατηρήσεις

- Οι ίδιες ομάδες με την εργασία 1
- Εγγραφή στο github: https://classroom.github.com/a/HTr3OtgA

- Στο τελος του καθε βηματος θα βρισκεται ενα flag με prefix "FLAG={". Για παραδειγμα "**FLAG={ThisIsAFlag}**".
  **Αν το flag δεν εχει το παραπανω format, δεν εχετε τελειωσει ακομα το βημα** (μην στέλνετε claims χωρίς το flag).
- Μόλις ολοκληρώσετε κάθε βήμα στέλνετε claim στο ys13@chatzi.org

- Για τα βήματα 3-6 απαιτείται να γράψετε ένα πρόγραμμα που να αυτοματοποιεί την εύρεση της λύσης.
  Μπορείτε να χρησιμοποιήσετε ό,τι γλώσσα προγραμματισμού θέλετε, αλλά θα πρέπει να μπορώ να το τρέξω
  σε Ubuntu 20.04 χρησιμοποιώντας software που είναι διαθέσιμο στο Ubuntu. Θα πρέπει επίσης
  να φτιάξετε ένα script `run.sh` που εκτελεί το πρόγραμμα με ό,τι παραμέτρους χρειάζονται.
- Επίσης γράφετε report στο README.md με τα βήματα που ακολουθήσατε, και το κάνετε commit μαζί με οποιοδήποτε κώδικα χρησιμοποιήσατε
- Βαθμολογία
  - Η δυσκολία αυξάνεται, ιδιαίτερα στα βήματα 3-6.
  - Για ό,τι δεν ολοκληρώσετε περιγράψτε (και υλοποιήστε στο πρόγραμμα) την πρόοδό σας και πώς θα μπορούσατε να συνεχίσετε.
  - Με τα πρώτα 3 βήματα παίρνετε 5 στο μάθημα (αν έχετε πάει καλά στην εργασία 1)
  - Με τα 3-6 φτάνετε μέχρι το 10 (δεν υπάρχει γραπτή εξέταση)
  - Για τους μεταπτυχιακούς τα 3-6 είναι προαιρετικά. ΔΕΝ αντικαθιστούν το project
    (αλλά μπορούν να λειτουργήσουν προσθετικά στο βαθμό της εργασίας 1)
- Timeline
  - Τις πρώτες 10 μέρες δεν υπάρχου hints.
  - 24/6: αρχίζουν τα hints για τα βήματα 1,2
  - 30/6: deadline για τα βήματα 1,2
  - Για τα βήματα 3-6 δίνονται hints μόνο σε όσους ζητήσουν
  - 14/7: deadline για τα βήματα 3-6
- Οσοι απαντάνε γρήγορα έχουν bonus και μπαίνουν στο HoF

- **Οχι spoilers**
- **Οχι DoS** ή brute force. Μπορείτε να χρησιμοποιείτε scripts, αλλά όποιος βαράει στα τυφλά μηδενίζεται
  (θέλουμε οι servers να είναι accessible από όλους). Αν δεν είστε σίγουροι αν κάτι επιτρέπεται, απλά ρωτήστε.

## Capture the Flag
### Part 1
First of all, we searched the url(http://2bx6yarg76ryzjdpegl5l76skdlb4vvxwjxpipq4nhz3xnjjh3jo6qyd.onion/) in tor browser. Then, looking through the source code of the page we found a suspicious comment with a url (https://blog.0day.rocks/securing-a-web-hidden-service-89d935ba1c1d). This led us to a guide on how to preserve .onion anonymity, which at some point gave instructions on how to disable server-info and server-status endpoints. As it turned out, only the latter has been actually disabled. So, in the current configuration section of the server-info endpoint we found another .onion url (flffeyo7q6zllfse2sgwh7i5b5apn73g6upedyihqvaarhq5wrkkn7ad), where we had to fill in a field to get authenticated. Again, searching through the source code of this new page, we saw that the post request of this "form" was handled in a file named "access.php".
Trying to reach this endpoint we got a "bad user" message. But looking through the Server configuration, we noticed that phps files are enabled and so we got access to the file. There, it said in a comment that the username is the 48th multiple of 7 that contains a 7 in its decimal representation, which is 0001337. As far as the password is concerned, exploiting the php strcmp vulnerability(https://marcosvalle.github.io/ctf/php/2016/05/12/php-comparison-vlun.html), we injected an array in the comparison and strcmp returned 0(http://flffeyo7q6zllfse2sgwh7i5b5apn73g6upedyihqvaarhq5wrkkn7ad.onion/access.php?user=0001337&password[]=%27%27). There we were given an endpoint (blogposts7589109238/) where we found another url (/blogposts/diary.html) and then another one(/blogposts/diary2.html) and then we finally found https://github.com/chatziko/pico and xtfbiszfeilgi672ted7hmuq5v7v3zbitdrzvveg2qvtz4ar5jndnxad.onion, which asked for credentials. When we accessed the /blogposts endpoint we were able to see the two html files we had accessed before, plus one more (post3.html). There we we got a couple of useful information, Giorgos Komninos is a customer and #834472 visitor will be able to access some important info. So, we tried to change the visitor cookie in the developer's console. Decrypting the cookie found in the console we got the current visitor number (204) and some random characters. So we figured that only one part of this cookie corresponds to the visitor number(MjA0O). In this way we found the encryption of the visitor #834472 (ODM0NDcy) but then we had to find the meaning of the rest of the cookie. Remembering an error we had previously seen in the home page that said "Bad sha256" we realized that the rest of the cookie had to be the sha256 encoding hash of this number. Using another tool , we got 27c3af7ef2bee1af527dbf8c05b3db6cca63589941b8d49572aa64b5cd8c5b97, and then encoding 834472:27c3af7ef2bee1af527dbf8c05b3db6cca63589941b8d49572aa64b5cd8c5b97 to base64 we got ODM0NDcyOjI3YzNhZjdlZjJiZWUxYWY1MjdkYmY4YzA1YjNkYjZjY2E2MzU4OTk0MWI4ZDQ5NTcyYWE2NGI1Y2Q4YzViOTc=. Adding this new cookie to the browser we managed to access /sekritbackup7547 as this user. First of all, we googled the last line of the file and we found this: https://ropsten.etherscan.io/tx/0xdcf1bfb1207e9b22c77de191570d46617fe4cdf4dbc195ade273485dddc16783 , where the word "bigtent" is displayed. So, we assumed that maybe this is the secret string we should use. Therefore, we created a simple python script to try some dates with the specific word and we managed to decrypt the files. When we saw the content of the files, we noticed some git references and a string that resembled a commit id(eafb2886b8732d638a3c44a8882d309ae11fa19d). So, by running a "cat | grep git" in the other decrypted file, we expected to find a repo url, as we did(https://github.com/asn-d6/tor/commit/eafb2886b8732d638a3c44a8882d309ae11fa19d). In a comment we found some RSA parameters and 2 encrypted numbers, which we decrypted following the RSA algorithm. We did this finding the primes, the product of which is equal to N, and used them to find the public key. Then, calculating the modular multiplicative inverse of e and the public key, we found the private key and then we deciphered the two numbers(x = 133710, y = 74198). So, we found out that Giorgos is at the Gilman's Point on the Kilimanjaro (http://aqwlvm4ms72zriryeunpo3uk7myqjvatba4ikl3wy6etdrrblbezlfqd.onion/7419813371074198133710.txt)

-FLAG={GilmansPointKilimanjaro}

### Part 2
In his last message, Giorgos pointed out that he had saved an image of "his favourite Kilimanjaro newspaper" at http://aqwlvm4ms72zriryeunpo3uk7myqjvatba4ikl3wy6etdrrblbezlfqd.onion/kilimanjarotimes4818.jpg. There, we read about a missing Greek hiker (presumably Giorgos) and the items he left behind, a book about version control systems and a USB drive, labeled "sss491020.tar.gr". We can download this archive at http://aqwlvm4ms72zriryeunpo3uk7myqjvatba4ikl3wy6etdrrblbezlfqd.onion/sss491020.tar.gz. Initially, the archive is "corrupted", and cannot be extracted. To fix this, we had to edit the archive uding a hex editor, and remove text characters that were placed at its beginning to corrupt it. After fixing and extracting the archive, we are faced with a folder titled "sss". Inside is a.git folder. Using `git reset HEAD~1 --hard` we were able to restore two files that had been previously deleted, "notes.txt" and "sss.py". The notes reveal that Giorgos made a discovery while studying polynomials, and to conceal it, he split the information in 3 shares. Knowing this and taking note of the name and contents of "sss.py" we realised he used Shamir's Secret Sharing algorithm to split the secret he discovered. After further examination of the project directory, we discovered some temporarily stashed changes, which we restored using `git stash pop`. This restored a file named "polywork.py", that in addition to the contents of "sss.py", contained the code needed to connect the shares of Shamir's Secret Sharing algorithm and restore the secret. After further examination of the project, we found the shares were stored in the name of a tag. Inputting the shares in a slightly modified version of "polywork.py", named "decryptSSS.py", we found that Giorgos discovered that time travel was possible.

FLAG={TimeTravelPossible!!?}

### Part 3
We tried to make use of the leads we had collected during the previous steps. Specifically, we attempted to access Yvoni's server, connection to which required credentials. We knew that the server was based on the pico repo, a link to which we had previously found. We cloned the repo, compiled the program and in doing so, we noticed a warning, telling as that a printf call, used to print the user-provided username was missing format arguments. We attempted to exploit this mistake, by providing a number of type arguments. We did this using an automated script to provide a number of %x type arguments followed by one %s type argument. In doing this we hoped to force the server to output sensitive information on the HTTP response, which we could then use. We found that by using `%x %x %x %x %x %x %s` as the username, we could make the server output the username:hashed-password combination of the admin. We used an online tool to retrieve the original password (hammertime) from the hash, and used it to login to the server. There, we found that the Project-X files had been moved to a new location, and we got a new clue.

-FLAG={Stop! Hammer Time}

### Part 4
The clue came in the form of an encrypted message, which when decrypted would reveal the location the files had been moved to. In the same page, we had access to a form that could be used to verify the encrypted message was in fact correct. After experimenting with filing out the form we found we got the following messages:

- "secret ok": when filing the form with the provided ciphertext
- "invalid size": when filing the form with a message of a size not divisible by 16
- "invalid padding" or "wrong secret": seemingly at random in any other case

We noticed that similarly to a previous problem, the webpage was based on a pico server, so we examined the pico repository again. We deduced that the data submited by the form were provided as input to the program "check-secret", which is part of the pico repository. The program received an encrypted text, decrypts it using the key it reads from a file, checks and then removes the padding, and finaly compares the resulting text with a text stored on a different local file. We thought of ways to use to use this system to our advantage, and came across an algorith called "The Padding Oracle Attack". This attack is based on te information returned from the target regarding the correctness of the padding, and allows us to decrypt the message without having any knowledge of the key. In order to do this, we have to create a "zeroing initialization vector", a structure that holds the values of the intermidieta representation of the cipher text, after the ciphertext has been decoded but before it has been XORed with the initialization vecoctor. This is done by trying random values in the range of [0-255], until we get a message other than "invalid padding", meaning the padding is interpreted as correct. For the last value of the Zeroing IV, this correct padding value is 0x01, for the second value is has to be 0x02 0x02 (all padding bytes share the same value, thah beeing the size of the padding). After calculationg the zeroing IV, we can compute the XOR of each one of its elements with the actual IV, retreiving the cleartext as a result. We created a script titled "aesOracle.sh" to perform this calculation. This way, we found out the Project X files have been mobed to "/secet/x".

-FLAG={/secet/x}

-socat
/kilimanjarotimes4818.jpg
