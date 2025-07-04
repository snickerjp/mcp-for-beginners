<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "2342baa570312086fc19edcf41320250",
  "translation_date": "2025-06-17T15:47:34+00:00",
  "source_file": "03-GettingStarted/02-client/README.md",
  "language_code": "el"
}
-->
Στον προηγούμενο κώδικα:

- Εισάγουμε τις βιβλιοθήκες
- Δημιουργούμε ένα στιγμιότυπο πελάτη και το συνδέουμε χρησιμοποιώντας stdio για τη μεταφορά.
- Λίστα με prompts, πόρους και εργαλεία και τα καλούμε όλα.

Και να το, ένας πελάτης που μπορεί να επικοινωνήσει με έναν MCP Server.

Ας πάρουμε τον χρόνο μας στην επόμενη ενότητα άσκησης και ας αναλύσουμε κάθε κομμάτι κώδικα και να εξηγήσουμε τι συμβαίνει.

## Άσκηση: Δημιουργία πελάτη

Όπως είπαμε παραπάνω, ας πάρουμε τον χρόνο μας να εξηγήσουμε τον κώδικα, και φυσικά, μπορείτε να γράψετε κώδικα παράλληλα αν θέλετε.

### -1- Εισαγωγή βιβλιοθηκών

Ας εισάγουμε τις βιβλιοθήκες που χρειαζόμαστε, θα χρειαστούμε αναφορές σε έναν πελάτη και στο επιλεγμένο πρωτόκολλο μεταφοράς, το stdio. Το stdio είναι ένα πρωτόκολλο για πράγματα που προορίζονται να τρέχουν στον τοπικό σας υπολογιστή. Το SSE είναι ένα άλλο πρωτόκολλο μεταφοράς που θα δείξουμε σε μελλοντικά κεφάλαια, αλλά αυτή είναι η άλλη σας επιλογή. Προς το παρόν, ας συνεχίσουμε με το stdio.

Ας προχωρήσουμε στην δημιουργία στιγμιοτύπου.

### -2- Δημιουργία στιγμιοτύπου πελάτη και μεταφοράς

Θα χρειαστεί να δημιουργήσουμε ένα στιγμιότυπο του πρωτοκόλλου μεταφοράς και ένα του πελάτη μας:

### -3- Λίστα χαρακτηριστικών του διακομιστή

Τώρα, έχουμε έναν πελάτη που μπορεί να συνδεθεί όταν τρέξει το πρόγραμμα. Ωστόσο, δεν εμφανίζει πραγματικά τα χαρακτηριστικά του, οπότε ας το κάνουμε αυτό τώρα:

Τέλεια, τώρα έχουμε καταγράψει όλα τα χαρακτηριστικά. Τώρα το ερώτημα είναι πότε τα χρησιμοποιούμε; Αυτός ο πελάτης είναι αρκετά απλός, απλός με την έννοια ότι θα χρειαστεί να καλέσουμε ρητά τα χαρακτηριστικά όταν τα θέλουμε. Στο επόμενο κεφάλαιο, θα δημιουργήσουμε έναν πιο προχωρημένο πελάτη που θα έχει πρόσβαση στο δικό του μεγάλο γλωσσικό μοντέλο, LLM. Προς το παρόν όμως, ας δούμε πώς μπορούμε να καλέσουμε τα χαρακτηριστικά στον διακομιστή:

### -4- Κλήση χαρακτηριστικών

Για να καλέσουμε τα χαρακτηριστικά, πρέπει να βεβαιωθούμε ότι ορίζουμε τα σωστά επιχειρήματα και σε ορισμένες περιπτώσεις το όνομα αυτού που προσπαθούμε να καλέσουμε.

### -5- Εκτέλεση πελάτη

Για να εκτελέσετε τον πελάτη, πληκτρολογήστε την ακόλουθη εντολή στο τερματικό:

## Ανάθεση

Σε αυτήν την ανάθεση, θα χρησιμοποιήσετε όσα μάθατε για τη δημιουργία ενός πελάτη, αλλά θα δημιουργήσετε τον δικό σας πελάτη.

Εδώ έχετε έναν διακομιστή που μπορείτε να χρησιμοποιήσετε και πρέπει να τον καλέσετε μέσω του κώδικα του πελάτη σας, δείτε αν μπορείτε να προσθέσετε περισσότερα χαρακτηριστικά στον διακομιστή για να τον κάνετε πιο ενδιαφέροντα.

## Λύση

[Solution](./solution/README.md)

## Κύρια σημεία

Τα βασικά σημεία αυτού του κεφαλαίου σχετικά με τους πελάτες είναι τα εξής:

- Μπορούν να χρησιμοποιηθούν τόσο για να ανακαλύψουν όσο και για να καλέσουν χαρακτηριστικά στον διακομιστή.
- Μπορούν να ξεκινήσουν έναν διακομιστή ενώ ξεκινούν και οι ίδιοι (όπως σε αυτό το κεφάλαιο), αλλά οι πελάτες μπορούν επίσης να συνδεθούν σε ήδη ενεργούς διακομιστές.
- Είναι ένας εξαιρετικός τρόπος για να δοκιμάσετε τις δυνατότητες του διακομιστή παράλληλα με εναλλακτικές όπως ο Inspector, όπως περιγράφηκε στο προηγούμενο κεφάλαιο.

## Πρόσθετοι πόροι

- [Building clients in MCP](https://modelcontextprotocol.io/quickstart/client)

## Παραδείγματα

- [Java Calculator](../samples/java/calculator/README.md)
- [.Net Calculator](../../../../03-GettingStarted/samples/csharp)
- [JavaScript Calculator](../samples/javascript/README.md)
- [TypeScript Calculator](../samples/typescript/README.md)
- [Python Calculator](../../../../03-GettingStarted/samples/python) 

## Τι ακολουθεί

- Επόμενο: [Δημιουργία πελάτη με LLM](/03-GettingStarted/03-llm-client/README.md)

**Αποποίηση Ευθυνών**:  
Αυτό το έγγραφο έχει μεταφραστεί χρησιμοποιώντας την υπηρεσία αυτόματης μετάφρασης AI [Co-op Translator](https://github.com/Azure/co-op-translator). Παρόλο που προσπαθούμε για ακρίβεια, παρακαλούμε να έχετε υπόψη ότι οι αυτόματες μεταφράσεις ενδέχεται να περιέχουν λάθη ή ανακρίβειες. Το πρωτότυπο έγγραφο στη μητρική του γλώσσα πρέπει να θεωρείται η αυθεντική πηγή. Για κρίσιμες πληροφορίες, συνιστάται η επαγγελματική ανθρώπινη μετάφραση. Δεν φέρουμε ευθύνη για οποιεσδήποτε παρεξηγήσεις ή λανθασμένες ερμηνείες προκύψουν από τη χρήση αυτής της μετάφρασης.