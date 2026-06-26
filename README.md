SV30

Kot34 - Utilizând să se scrie un program Kotlin care preia un fișier text (scris de programator) la intrare, organizează cuvintele într-un arbore binar utilizând o constrângere la alegere, iar apoi cu ajutorul unei clase specifice și al funcțiilor va afișa porțiuni din arbore.

Pyth361 - Să se creeze o aplicație Python pentru rezervarea unei camere de hostel. Camerele (cu atributele și operatorii lor) sunt obiecte și sunt reținute într-un arbore. Cu ajutorul unui decorator implementați OOP peste funcțiile de bază și se vor implementa un flux suplimentar de tratare al plății următorului serviciu suplimentar: consum din barul camerei când se generează nota de plată. Se vor prezenta și diagramele de clase și obiecte. Se va explica maniera de aplicare a principiilor SOLID.

kotlin 34

import java.io.File

// =======================
// Nod din arbore
// =======================


class Nod(
    val cuvant: String
) {
    var stanga: Nod? = null
    var dreapta: Nod? = null
}


// =======================
// Arbore binar
// =======================
class Arbore {

    var radacina: Nod? = null

    // Adaugam un cuvant in arbore
    fun adauga(cuvant: String) {

        val nodNou = Nod(cuvant)

        // Daca arborele este gol
        if (radacina == null) {
            radacina = nodNou
            return
        }

        var curent = radacina

        while (true) {

            // Constrangerea aleasa:
            // mai mic -> stanga
            // mai mare -> dreapta
            if (cuvant < curent!!.cuvant) {

                if (curent.stanga == null) {
                    curent.stanga = nodNou
                    return
                }

                curent = curent.stanga
            }
            else if (cuvant > curent.cuvant) {

                if (curent.dreapta == null) {
                    curent.dreapta = nodNou
                    return
                }

                curent = curent.dreapta
            }
            else {
                // daca exista deja
                return
            }
        }
    }

    // Afisare in ordine alfabetica
    fun afisare(nod: Nod?) {

        if (nod == null)
            return

        afisare(nod.stanga)

        println(nod.cuvant)

        afisare(nod.dreapta)
    }

    // Afisam doar cuvintele
    // care incep cu o litera
    fun afisareLitera(
        nod: Nod?,
        litera: Char
    ) {

        if (nod == null)
            return

        afisareLitera(
            nod.stanga,
            litera
        )

        if (nod.cuvant.startsWith(litera))
            println(nod.cuvant)

        afisareLitera(
            nod.dreapta,
            litera
        )
    }
}


// =======================
// Main
// =======================
fun main() {

    val arbore = Arbore()

    // Citim textul din fisier
    val text =
        File("text.txt").readText()

    // Impartim textul in cuvinte
    val cuvinte =
        text.split(" ")

    // Adaugam fiecare cuvant
    for (cuvant in cuvinte) {
        arbore.adauga(cuvant)
    }

    println("Toate cuvintele:")
    arbore.afisare(
        arbore.radacina
    )

    println()
    println("Cuvinte care incep cu a:")

    arbore.afisareLitera(
        arbore.radacina,
        'a'
    )
}

python 361

# =====================================================
# CLASA CAMERA
# =====================================================
# O camera are:
# - numar
# - tip
# - pret pe noapte
# - consum din bar
class Camera:

    def __init__(self, numar, tip, pret):

        self.numar = numar
        self.tip = tip
        self.pret = pret

        # Initial nu s-a consumat nimic din bar
        self.consum_bar = 0


# =====================================================
# ADT PENTRU CAMERE
# =====================================================
# ADT-ul tine toate camerele hotelului
# intr-o lista.
class ADTCamere:

    def __init__(self):

        # Lista de camere
        self.camere = []

    # Adaugam o camera
    def adauga_camera(self, camera):

        self.camere.append(camera)

    # Cautam o camera dupa numar
    def cauta_camera(self, numar):

        for camera in self.camere:

            if camera.numar == numar:
                return camera

        # Daca nu exista
        return None

    # Afisam toate camerele
    def afiseaza_camere(self):

        for camera in self.camere:

            print(
                "Camera",
                camera.numar,
                "tip",
                camera.tip,
                "pret/noapte",
                camera.pret,
                "lei"
            )


# =====================================================
# DECORATOR
# =====================================================
# Decoratorul adauga consumul din bar
# peste pretul normal al cazarii.
def decorator_consum_bar(functie):

    def wrapper(rezervare):

        # Calculam pretul cazarii
        total = functie(rezervare)

        # Adaugam consumul din bar
        total = (
                total +
                rezervare.camera.consum_bar
        )

        return total

    return wrapper


# =====================================================
# CLASA REZERVARE
# =====================================================
class Rezervare:

    def __init__(
            self,
            client,
            camera,
            nr_nopti
    ):

        self.client = client
        self.camera = camera
        self.nr_nopti = nr_nopti

    # Calculul pretului este decorat
    # cu consumul din bar.
    @decorator_consum_bar
    def calculeaza_total(self):

        return (
                self.camera.pret *
                self.nr_nopti
        )

    def afiseaza_nota_plata(self):

        print()
        print("===== NOTA DE PLATA =====")

        print(
            "Client:",
            self.client
        )

        print(
            "Camera:",
            self.camera.numar
        )

        print(
            "Tip:",
            self.camera.tip
        )

        print(
            "Pret/noapte:",
            self.camera.pret,
            "lei"
        )

        print(
            "Numar nopti:",
            self.nr_nopti
        )

        print(
            "Consum bar:",
            self.camera.consum_bar,
            "lei"
        )

        print(
            "TOTAL:",
            self.calculeaza_total(),
            "lei"
        )


# =====================================================
# PROGRAM PRINCIPAL
# =====================================================
def main():

    # Cream ADT-ul
    hotel = ADTCamere()

    # Cream cateva camere
    camera1 = Camera(
        101,
        "Single",
        200
    )

    camera2 = Camera(
        102,
        "Double",
        300
    )

    camera3 = Camera(
        103,
        "Apartament",
        500
    )

    # Adaugam camerele in hotel
    hotel.adauga_camera(camera1)
    hotel.adauga_camera(camera2)
    hotel.adauga_camera(camera3)

    print("Camere disponibile:")
    hotel.afiseaza_camere()

    print()

    # Cautam camera 102
    camera_aleasa = hotel.cauta_camera(102)

    # Verificam daca exista
    if camera_aleasa is not None:

        # Clientul a consumat din bar
        camera_aleasa.consum_bar = 75

        # Cream rezervarea
        rezervare = Rezervare(
            "Ion Popescu",
            camera_aleasa,
            3
        )

        # Afisam nota de plata
        rezervare.afiseaza_nota_plata()

    else:
        print(
            "Camera nu exista."
        )


if __name__ == "__main__":
    main()




S59

Kot-042 – Să se scrie un program Kotlin care creează o corutină care aplică o transformare lambda de tipul f(x)=x+3 peste elementele de index impar ale unui ADT primit ca parametru utilizând monadele.

Pyt-066 – Utilizând modelul mediator și clasele ABC să se creeze un program care să permită comunicarea prin mesaje între niște obiecte de tip furnică (ex. mesaje: mâncare, drum bun, pericol, ajutor). Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


kotlin 042

import kotlinx.coroutines.*

fun main() = runBlocking {

    // =================================================
    // ADT-ul
    // =================================================
    // Folosim o lista mutabila.
    // Este mutabila deoarece vrem sa modificam valorile din ea.
    val lista = mutableListOf(
        2,
        4,
        6,
        8,
        1,
        7
    )

    println("Lista initiala:")
    println(lista)

    // =================================================
    // Functia lambda
    // =================================================
    // Aceasta este transformarea ceruta in enunt:
    //
    // f(x) = x + 3
    //
    // Primeste un Int si returneaza un Int.
    val transformare: (Int) -> Int = { x ->
        x + 3
    }

    // =================================================
    // Corutina
    // =================================================
    // launch porneste o corutina.
    // Corutina executa blocul de cod dintre acolade.
    //
    // launch nu returneaza un rezultat,
    // ci doar executa o actiune.
    launch {

        // =================================================
        // Procesare cu functii pe colectii
        // =================================================
        // lista.indices inseamna toti indicii listei.
        //
        // Pentru lista:
        // [2, 4, 6, 8, 1, 7]
        //
        // indicii sunt:
        // 0, 1, 2, 3, 4, 5
        lista.indices

            // filter pastreaza doar indicii impari.
            //
            // it reprezinta fiecare index.
            //
            // it % 2 == 1 inseamna:
            // indexul este impar.
            //
            // Raman:
            // 1, 3, 5
            .filter { it % 2 == 1 }

            // forEach parcurge indicii ramasi.
            //
            // it va fi pe rand:
            // 1, apoi 3, apoi 5
            .forEach {

                // Modificam doar elementele aflate
                // pe pozitii impare.
                //
                // lista[it] este valoarea de la indexul it.
                //
                // Aplicam lambda:
                // transformare(x) = x + 3
                lista[it] = transformare(lista[it])
            }

    }.join()
    // join() asteapta terminarea corutinei.
    // Fara join, programul ar putea continua
    // inainte ca modificarea sa fie finalizata.

    println()
    println("Lista finala:")
    println(lista)
}

python 066

from abc import ABC, abstractmethod


# =====================================================
# INTERFATA MEDIATOR
# =====================================================
# ABC inseamna clasa abstracta.
# Aceasta clasa spune ca orice mediator trebuie sa aiba
# metoda trimite_mesaj().
class Mediator(ABC):

    @abstractmethod
    def trimite_mesaj(self, mesaj, expeditor):
        pass


# =====================================================
# MEDIATOR CONCRET
# =====================================================
# ChatMusuroi este mediatorul real.
# El tine lista furnicilor si distribuie mesajele.
class ChatMusuroi(Mediator):

    def __init__(self):
        self.furnici = []

    # Adaugam o furnica in lista mediatorului
    def adauga_furnica(self, furnica):
        self.furnici.append(furnica)

    # Primeste un mesaj de la o furnica
    # si il trimite tuturor celorlalte furnici.
    def trimite_mesaj(self, mesaj, expeditor):

        print()
        print(f"{expeditor.nume} trimite mesajul: {mesaj}")

        for furnica in self.furnici:

            # Nu trimitem mesajul inapoi
            # furnicii care l-a trimis.
            if furnica != expeditor:
                furnica.primeste_mesaj(mesaj, expeditor)


# =====================================================
# CLASA FURNICA
# =====================================================
# Furnica poate trimite si primi mesaje.
# Ea nu cunoaste celelalte furnici.
# Ea cunoaste doar mediatorul.
class Furnica:

    def __init__(self, nume, mediator):
        self.nume = nume
        self.mediator = mediator

        # Cand cream furnica,
        # o inregistram automat in mediator.
        self.mediator.adauga_furnica(self)

    # Furnica trimite mesajul catre mediator
    def spune(self, mesaj):
        self.mediator.trimite_mesaj(mesaj, self)

    # Furnica primeste mesajul de la mediator
    def primeste_mesaj(self, mesaj, expeditor):
        print(
            f"{self.nume} a primit de la "
            f"{expeditor.nume}: {mesaj}"
        )

    def __str__(self):
        return self.nume


# =====================================================
# PROGRAM PRINCIPAL
# =====================================================
def main():

    # Cream mediatorul
    chat = ChatMusuroi()

    # Cream furnicile.
    # Toate primesc acelasi mediator.
    furnica1 = Furnica("Furnica Mica", chat)
    furnica2 = Furnica("Furni", chat)
    furnica3 = Furnica("Furnicuta", chat)
    furnica4 = Furnica("Soldat", chat)

    # Furnicile trimit mesaje.
    furnica1.spune("Am gasit mancare!")
    furnica2.spune("Drum bun!")
    furnica3.spune("Pericol!")
    furnica4.spune("Ajutor!")


if __name__ == "__main__":
    main()




SV33

Kot39 – Utilizând Kotlin funcțiile pentru procesarea submulțimilor dintr-un fișier text (creat de programator cu câteva propoziții în el) să se vor extrage două caractere din mijlocul cuvântului dacă cuvântul are minim patru caractere. Se va utiliza combinație cu lambda peste colecții pentru procesare.

Pyth341 – Utilizând modelul mediator și să se creeze un program Python care să permită comunicarea prin mesaje între niște obiecte de tip furnică (e.g. mesaje: mâncare, drum bun, pericol, ajutor). Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


1.

// Importam clasa File pentru a putea citi un fisier text
import java.io.File

fun main() {

    // ============================================
    // CITIREA FISIERULUI
    // ============================================

    // Citim TOT continutul fisierului text.txt
    // De exemplu:
    // Astazi invat Kotlin foarte mult
    val text = File("text.txt").readText()

    // Afisam textul citit
    println("Textul din fisier:")
    println(text)
    println()


    // ============================================
    // IMPARTIREA TEXTULUI IN CUVINTE
    // ============================================

    // split(" ")
    // desparte textul dupa spatii
    //
    // Exemplu:
    // "Astazi invat Kotlin"
    //
    // devine:
    // ["Astazi", "invat", "Kotlin"]

    val cuvinte = text.split(" ")

    println("Cuvintele sunt:")
    println(cuvinte)
    println()


    // ============================================
    // PRELUCRAREA COLECTIEI
    // ============================================

    val rezultat = cuvinte

        // ==================================================
        // FILTER
        // ==================================================
        // Pastreaza doar cuvintele care au
        // minim 4 caractere.
        //
        // it reprezinta fiecare cuvant.
        //
        // Exemplu:
        //
        // it = "Astazi"
        // it.length = 6
        //
        // 6 >= 4 -> ADEVARAT
        //
        // cuvantul ramane in colectie.
        .filter {
            it.length >= 4
        }

        // ==================================================
        // MAP
        // ==================================================
        // Transforma fiecare cuvant
        // in cele doua litere din mijloc.
        //
        // Exemplu:
        //
        // Astazi
        // 012345
        //
        // lungime = 6
        // mijloc = 3
        //
        // substring(2,4)
        // => "ta"
        .map {

            // Calculam pozitia de mijloc
            val mijloc = it.length / 2

            // Extragem cele doua caractere
            // din mijloc.
            //
            // substring(start, end)
            //
            // end NU este inclus.
            it.substring(
                mijloc - 1,
                mijloc + 1
            )
        }


    // ============================================
    // AFISAREA REZULTATULUI
    // ============================================

    println("Cele doua caractere din mijloc:")

    // forEach parcurge colectia rezultat.
    //
    // it reprezinta fiecare element
    // din lista rezultat.
    rezultat.forEach {

        println(it)
    }
}

2.

# =====================================================
# CLASA MEDIATOR
# =====================================================
# Mediatorul este obiectul central prin care comunica furnicile.
# Furnicile NU trimit mesaje direct una catre alta.
class MediatorFurnici:

    def __init__(self):

        # Lista cu toate furnicile inregistrate
        self.furnici = []

    # Adaugam o furnica in mediator
    def adauga_furnica(self, furnica):

        self.furnici.append(furnica)

    # Trimite mesajul catre toate furnicile,
    # mai putin catre furnica ce a trimis mesajul.
    def trimite_mesaj(self, expeditor, mesaj):

        print()
        print(f"{expeditor.nume} trimite mesajul: {mesaj}")

        for furnica in self.furnici:

            # Nu trimitem mesajul inapoi la expeditor
            if furnica != expeditor:

                furnica.primeste_mesaj(
                    expeditor,
                    mesaj
                )


# =====================================================
# CLASA FURNICA
# =====================================================
# Furnica este un obiect care poate trimite si primi mesaje.
class Furnica:

    def __init__(self, nume, mediator):

        # Numele furnicii
        self.nume = nume

        # Mediatorul prin care comunica
        self.mediator = mediator

        # Cand cream furnica,
        # o inregistram in mediator.
        self.mediator.adauga_furnica(self)

    # Furnica trimite mesajul prin mediator
    def trimite_mesaj(self, mesaj):

        self.mediator.trimite_mesaj(
            self,
            mesaj
        )

    # Furnica primeste mesajul
    def primeste_mesaj(self, expeditor, mesaj):

        print(
            f"{self.nume} a primit de la "
            f"{expeditor.nume}: {mesaj}"
        )


# =====================================================
# PROGRAM PRINCIPAL
# =====================================================

def main():

    # Cream mediatorul
    mediator = MediatorFurnici()

    # Cream furnicile
    furnica1 = Furnica(
        "Furnica 1",
        mediator
    )

    furnica2 = Furnica(
        "Furnica 2",
        mediator
    )

    furnica3 = Furnica(
        "Furnica 3",
        mediator
    )

    furnica4 = Furnica(
        "Furnica 4",
        mediator
    )

    # Furnicile trimit mesaje
    furnica1.trimite_mesaj("Am gasit mancare!")

    furnica2.trimite_mesaj("Drum bun spre musuroi.")

    furnica3.trimite_mesaj("Pericol in apropiere!")

    furnica4.trimite_mesaj("Am nevoie de ajutor!")


# Pornirea programului
if __name__ == "__main__":
    main()







SV80

Kot70 – Utilizând modelul adaptor să se creeze un program Kotlin care va primi la intrare diverse tipuri de date simple sau de tip colecție și va realiza afișarea lor ca o singură operație care primește chiar obiectul. Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.

Pyt81 – Utilizând modelul command să se scrie în Python un program care pornind de la o clasă student va asigura posibilitatea unei comenzi specifice unui obiect profesor care să conducă la schimbarea stării interne a unui obiect student (de ex. din fericit în disperat). Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.



1.

// Interfata Target
// Aceasta este interfata pe care o foloseste clientul.
// Clientul stie doar ca poate apela metoda afiseaza().
interface Target {

    fun afiseaza(obiect: Any?)
}


// Adapterul
// El adapteaza mai multe tipuri de date
// si le afiseaza printr-o singura metoda.
class AfisareAdapter : Target {

    override fun afiseaza(obiect: Any?) {

        when (obiect) {

            // Daca obiectul este String
            is String -> {
                println("String: $obiect")
            }

            // Daca obiectul este Int
            is Int -> {
                println("Int: $obiect")
            }

            // Daca obiectul este Double
            is Double -> {
                println("Double: $obiect")
            }

            // Daca obiectul este Boolean
            is Boolean -> {
                println("Boolean: $obiect")
            }

            // Daca obiectul este List
            is List<*> -> {
                println("Lista:")
                for (element in obiect) {
                    println(element)
                }
            }

            // Daca obiectul este Set
            is Set<*> -> {
                println("Set:")
                for (element in obiect) {
                    println(element)
                }
            }

            // Daca obiectul este Map
            is Map<*, *> -> {
                println("Map:")
                for ((cheie, valoare) in obiect) {
                    println("$cheie -> $valoare")
                }
            }

            // Daca obiectul este null
            null -> {
                println("Obiect null")
            }

            // Daca este alt tip necunoscut
            else -> {
                println("Tip de date necunoscut: $obiect")
            }
        }
    }
}


// Clientul
// Clientul nu stie exact ce tip de date primeste.
// El stie doar ca are un adapter cu metoda afiseaza().
class Client(
    private val adapter: Target
) {

    fun proceseaza(obiect: Any?) {
        adapter.afiseaza(obiect)
    }
}


// Program principal
fun main() {

    // Cream adapterul
    val adapter = AfisareAdapter()

    // Cream clientul
    val client = Client(adapter)

    // Date simple
    client.proceseaza("Salut Kotlin")
    client.proceseaza(100)
    client.proceseaza(25.5)
    client.proceseaza(true)

    println()

    // Lista
    val lista = listOf("Ana", "Maria", "Ion")
    client.proceseaza(lista)

    println()

    // Set
    val set = setOf(1, 2, 3, 4)
    client.proceseaza(set)

    println()

    // Map
    val dictionar = mapOf(
        "Ana" to 10,
        "Ion" to 8,
        "Maria" to 9
    )

    client.proceseaza(dictionar)
}

2.

# =====================================================
# CLASA STUDENT
# =====================================================
# Studentul este obiectul asupra caruia se aplica comenzile.
# El are o stare interna.
class Student:

    def __init__(self, nume):

        # Numele studentului
        self.nume = nume

        # Starea initiala a studentului
        self.stare = "fericit"

    # Metoda care schimba starea studentului
    def schimba_stare(self, stare_noua):

        self.stare = stare_noua

    # Metoda care afiseaza starea studentului
    def afiseaza_stare(self):

        print(
            f"Studentul {self.nume} este {self.stare}."
        )


# =====================================================
# INTERFATA COMANDA
# =====================================================
# Orice comanda trebuie sa aiba metoda executa().
class Comanda:

    def executa(self):
        pass


# =====================================================
# COMANDA CONCRETA 1
# =====================================================
# Aceasta comanda face studentul disperat.
class ComandaDisperat(Comanda):

    def __init__(self, student):

        # Retinem studentul asupra caruia se aplica comanda
        self.student = student

    def executa(self):

        # Schimbam starea studentului
        self.student.schimba_stare("disperat")


# =====================================================
# COMANDA CONCRETA 2
# =====================================================
# Aceasta comanda face studentul fericit.
class ComandaFericit(Comanda):

    def __init__(self, student):

        self.student = student

    def executa(self):

        self.student.schimba_stare("fericit")


# =====================================================
# COMANDA CONCRETA 3
# =====================================================
# Aceasta comanda face studentul obosit.
class ComandaObosit(Comanda):

    def __init__(self, student):

        self.student = student

    def executa(self):

        self.student.schimba_stare("obosit")


# =====================================================
# CLASA PROFESOR
# =====================================================
# Profesorul nu modifica direct studentul.
# Profesorul primeste o comanda si o executa.
class Profesor:

    def __init__(self, nume):

        self.nume = nume

    def trimite_comanda(self, comanda):

        print(f"Profesorul {self.nume} trimite o comanda.")

        comanda.executa()


# =====================================================
# PROGRAM PRINCIPAL
# =====================================================
def main():

    # Cream studentul
    student = Student("Ion")

    # Afisam starea initiala
    student.afiseaza_stare()

    print()

    # Cream profesorul
    profesor = Profesor("Popescu")

    # Cream comenzile
    comanda_disperat = ComandaDisperat(student)
    comanda_fericit = ComandaFericit(student)
    comanda_obosit = ComandaObosit(student)

    # Profesorul trimite comanda de disperare
    profesor.trimite_comanda(comanda_disperat)
    student.afiseaza_stare()

    print()

    # Profesorul trimite comanda de oboseala
    profesor.trimite_comanda(comanda_obosit)
    student.afiseaza_stare()

    print()

    # Profesorul trimite comanda de fericire
    profesor.trimite_comanda(comanda_fericit)
    student.afiseaza_stare()


if __name__ == "__main__":
    main()




SV50

Kot 62 – Pornind de la două mulțimi A și B care conțin 20 de elemente depuse în două colecții separate și ținând cont de A×B={(a,b)∣a∈A∧b∈B}, să se scrie un program Kotlin care va calcula {Ai reunit cu Bi
} utilizând funcții specifice colecțiilor și eventual lambda calculului următoare. Rezultatul este depus într-un dicționar care apoi va fi afișat.

Pyth335 – Utilizând modelul observator să se creeze în Python pentru un obiect de tip recalculare a prețului, în funcție de niște rate de reducere introduse extern de la tastatură (prin intermediul unui model proxy cu validare pe user-parolă), un logger (scriere automată în jurnal a operațiilor) care va scrie într-un fișier separat user și data la care a fost efectuată modificarea de preț și ce modificare a fost realizată. Se vor desena diagrame de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.



1.

// Genereaza o multime cu 20 de numere aleatoare
fun genereazaMultime(): Set<Int> {

    val multime = mutableSetOf<Int>()

    // Adaugam numere pana cand avem 20 de elemente diferite
    while (multime.size < 20) {
        multime.add((1..50).random())
    }

    return multime
}


// Calculeaza produsul cartezian dintre doua multimi
fun produsCartezian(
    prima: Set<Int>,
    aDoua: Set<Int>
): List<Pair<Int, Int>> {

    val rezultat = mutableListOf<Pair<Int, Int>>()

    // Pentru fiecare element din prima multime
    for (x in prima) {

        // Il combinam cu fiecare element din a doua multime
        for (y in aDoua) {

            // Formam perechea (x, y)
            rezultat.add(Pair(x, y))
        }
    }

    return rezultat
}


// Afiseaza perechile
fun afiseazaPerechi(perechi: List<Pair<Int, Int>>) {

    for (pereche in perechi) {
        println("(${pereche.first}, ${pereche.second})")
    }
}


fun main() {

    // Cream cele doua multimi A si B
    val A = genereazaMultime()
    val B = genereazaMultime()

    println("Multimea A:")
    println(A)

    println()

    println("Multimea B:")
    println(B)

    println()

    // A ∪ B = reuniunea
    val reuniune = A.union(B)

    // B ∩ A = intersectia
    val intersectie = B.intersect(A)

    println("A ∪ B:")
    println(reuniune)

    println()

    println("B ∩ A:")
    println(intersectie)

    println()

    // Calculam (A ∪ B) × (B ∩ A)
    val rezultat = produsCartezian(
        reuniune,
        intersectie
    )

    println("(A ∪ B) x (B ∩ A):")
    afiseazaPerechi(rezultat)

    println()
    println("Numar de perechi: ${rezultat.size}")
}

2.

# ==========================================
# CLASA OBSERVER
# ==========================================

# Orice observator trebuie sa aiba metoda update().
class Observer:

    def update(self, mesaj):
        pass


# ==========================================
# CLASA LOGGER
# ESTE OBSERVERUL NOSTRU
# ==========================================

class Logger(Observer):

    # Aceasta functie se executa atunci cand
    # pretul laptopului se modifica.
    def update(self, mesaj):

        # Deschidem fisierul in modul "a"
        # (append = adaugare la sfarsit).
        with open("log.txt", "a") as fisier:
            fisier.write(mesaj + "\n")

        print("LOG:", mesaj)


# ==========================================
# CLASA LAPTOP
# ESTE SUBJECT-UL DIN OBSERVER
# ==========================================

class Laptop:

    # Constructor
    def __init__(self, nume, pret):

        self.nume = nume
        self.pret = pret

        # Lista de observatori
        self.observers = []

    # Adaugam un observator
    def attach(self, observer):

        self.observers.append(observer)

    # Notificam toti observatorii
    def notify(self, mesaj):

        for observer in self.observers:
            observer.update(mesaj)

    # Modificam pretul laptopului
    def modifica_pret(self, pret_nou):

        self.pret = pret_nou

        mesaj = (
            f"Pretul laptopului "
            f"{self.nume} "
            f"a devenit "
            f"{self.pret} lei."
        )

        # Anuntam observatorii
        self.notify(mesaj)


# ==========================================
# CLASA PROXY
# ==========================================

class LaptopProxy:

    # Primeste obiectul real Laptop
    def __init__(self, laptop):

        self.laptop = laptop

    # Modifica pretul doar daca parola este buna
    def modifica_pret(
            self,
            parola,
            pret_nou
    ):

        # Verificam parola
        if parola == "1234":

            # Daca este corecta,
            # apelam obiectul real
            self.laptop.modifica_pret(
                pret_nou
            )

        else:
            print("Parola gresita!")


# ==========================================
# PROGRAM PRINCIPAL
# ==========================================

def main():

    # Cream laptopul
    laptop = Laptop(
        "Asus",
        3500
    )

    # Cream logger-ul
    logger = Logger()

    # Atasam logger-ul la laptop
    laptop.attach(logger)

    # Cream proxy-ul
    proxy = LaptopProxy(laptop)

    print()

    # Parola corecta
    proxy.modifica_pret(
        "1234",
        4000
    )

    print()

    # Parola gresita
    proxy.modifica_pret(
        "1111",
        5000
    )

    print()

    # Alta modificare
    proxy.modifica_pret(
        "1234",
        4200
    )


# Pornirea programului
if __name__ == "__main__":
    main()


SV44

Kot57 – Să se scrie un program Kotlin care, având la intrare două mulțimi A și B de câte 15 numere inițializate aleator care vor fi depuse în niște colecții, să se aplice utilizând calculul lambda și operatorii specifici următoarea transformare:

A×B={(a,b)∣a∈A∧b∈B}

iar rezultatul trebuie la rândul lui depus într-o colecție și aceasta afișată.

Pyth359 – Să se creeze o aplicație Python pentru rezervarea unei camere de hostel. Camerele (cu atributele și operatorii lor) sunt obiecte și sunt reținute într-un arbore. Cu ajutorul unor decoratori peste funcțiile de bază se vor implementa un flux suplimentar de tratare a plății următorului serviciu suplimentar: consum din barul camerei când se generează nota de plată. Se vor prezenta și diagramele de clase și obiecte


1.

// Functie care genereaza o lista cu 15 numere aleatoare
fun genereazaMultime(): List<Int> {

    // List(15) inseamna ca vrem 15 elemente
    // (1..100).random() genereaza un numar intre 1 si 100
    return List(15) {
        (1..100).random()
    }
}


// Functie care calculeaza produsul cartezian A x B
fun produsCartezian(
    A: List<Int>,
    B: List<Int>
): List<Pair<Int, Int>> {

    // Lista in care vom pune perechile (a,b)
    val rezultat = mutableListOf<Pair<Int, Int>>()

    // Parcurgem fiecare element din A
    for (a in A) {

        // Pentru fiecare element din A
        // parcurgem toate elementele din B
        for (b in B) {

            // Formam perechea (a,b)
            val pereche = Pair(a, b)

            // Adaugam perechea in lista
            rezultat.add(pereche)
        }
    }

    // Returnam lista cu toate perechile
    return rezultat
}


// Functie pentru afisare
fun afiseazaLista(
    lista: List<Pair<Int, Int>>
) {

    for (pereche in lista) {

        println(
            "(${pereche.first}, ${pereche.second})"
        )
    }
}


fun main() {

    // Generam cele doua multimi
    val A = genereazaMultime()
    val B = genereazaMultime()

    println("Multimea A:")
    println(A)

    println()

    println("Multimea B:")
    println(B)

    println()

    // Calculam produsul cartezian
    val rezultat =
        produsCartezian(
            A,
            B
        )

    println("Produsul cartezian A x B:")

    afiseazaLista(rezultat)

    println()

    println(
        "Numar de perechi: ${rezultat.size}"
    )
}

2.

# Importam wraps.
# Acesta pastreaza numele si informatiile functiei decorate.
from functools import wraps


# =====================================================
# CLASA CAMERA
# =====================================================
# Reprezinta o camera de hotel.
class Camera:

    # Constructorul clasei.
    def __init__(self, numar, tip, pret):

        # Numarul camerei.
        self.numar = numar

        # Tipul camerei.
        # Exemple:
        # Single, Double, Apartament.
        self.tip = tip

        # Pretul pentru o noapte.
        self.pret = pret

        # Consumul din barul camerei.
        # Initial este 0.
        self.bar_consum = 0

    # Metoda de afisare.
    def __str__(self):
        return (
            f"Camera {self.numar}, "
            f"tip {self.tip}, "
            f"pret/noapte {self.pret} RON"
        )


# =====================================================
# ADT PENTRU CAMERE
# =====================================================
# Aceasta clasa retine mai multe obiecte Camera.
class ADTCamere:

    def __init__(self):

        # Lista de camere.
        self.camere = []

    # Adauga o camera in lista.
    def adauga_camera(self, camera):
        self.camere.append(camera)

    # Cauta o camera dupa numar.
    def cauta_camera(self, numar):

        for camera in self.camere:

            if camera.numar == numar:
                return camera

        return None

    # Afiseaza toate camerele.
    def afiseaza_camere(self):

        for camera in self.camere:
            print(camera)


# =====================================================
# DECORATORUL
# =====================================================
# Adauga consumul din bar peste pretul cazarii.
def cu_bar_decorator(functie):

    # wrapper este noua functie.
    # Ea va inlocui functia originala.
    @wraps(functie)
    def wrapper(rezervare):

        # Apelam functia originala.
        # Aceasta calculeaza doar:
        #
        # pret/noapte * numar_zile
        pret_total = functie(rezervare)

        # Adaugam consumul din bar.
        pret_total += rezervare.camera.bar_consum

        # Returnam noul pret.
        return pret_total

    # Returnam functia noua.
    return wrapper


# =====================================================
# CLASA REZERVARE
# =====================================================
class Rezervare:

    def __init__(
        self,
        camera,
        nume_client,
        zile
    ):

        # Camera rezervata.
        self.camera = camera

        # Numele clientului.
        self.nume_client = nume_client

        # Numarul de zile.
        self.zile = zile

    # Aplicam decoratorul.
    # El va adauga consumul din bar.
    @cu_bar_decorator
    def calcul_pret_total(self):

        # Pretul de baza:
        # pret/noapte * numar_zile
        return (
            self.camera.pret *
            self.zile
        )

    # Afisarea rezervarii.
    def __str__(self):

        return (
            f"Rezervare pentru "
            f"{self.nume_client}\n"

            f"{self.camera}\n"

            f"Numar zile: "
            f"{self.zile}\n"

            f"Consum bar: "
            f"{self.camera.bar_consum} RON\n"

            f"Pret total: "
            f"{self.calcul_pret_total()} RON"
        )


# =====================================================
# PROGRAM PRINCIPAL
# =====================================================
def main():

    # Cream ADT-ul.
    hotel = ADTCamere()

    # Cream camerele.
    camera1 = Camera(
        101,
        "Single",
        200
    )

    camera2 = Camera(
        102,
        "Double",
        300
    )

    camera3 = Camera(
        103,
        "Apartament",
        500
    )

    # Adaugam camerele in ADT.
    hotel.adauga_camera(camera1)
    hotel.adauga_camera(camera2)
    hotel.adauga_camera(camera3)

    print("Camere disponibile:")
    hotel.afiseaza_camere()

    print()

    # Cautam camera 102.
    camera_rezervata = hotel.cauta_camera(102)

    # Verificam daca exista.
    if camera_rezervata is not None:

        # Clientul a consumat din bar.
        camera_rezervata.bar_consum = 75

        # Cream rezervarea.
        rezervare = Rezervare(
            camera_rezervata,
            "Ion Popescu",
            3
        )

        print("NOTA DE PLATA:")
        print(rezervare)

    else:
        print("Camera nu exista.")


# Punctul de pornire al programului.
if __name__ == "__main__":
    main()



SV38

Kot52 – Utilizând ADT-ul (ovenul lambda) și transformări specifice colecțiilor având la intrare mulțimea:

A={1,2,3,…,100}

să se scrie un program Kotlin care va determina numărul submulțimilor cu două elemente ale mulțimii A, care conțin cel puțin o componentă impară.

Pyth346 – Utilizând tkinter și funcții lambda să se creeze un program Python care va afișa în eticheta ferestrei operațiile realizate cu mouse-ul în momentul în care facem un desen simplu (de ex. trag un triunghi din linii) pe un canvas din acea fereastră. Programul va avea meniu.



1.

// Functie care calculeaza C(n,k)
fun combinari(n: Long, k: Long): Long {

    var rezultat = 1L

    for (i in 1..k) {
        rezultat = rezultat * (n - i + 1) / i
    }

    return rezultat
}


fun main() {

    // Construim multimea A={1,2,...,100}
    val A = (1..100).toList()

    // Scoatem elementul 1 deoarece acesta
    // este deja ales in submultime
    val elementeRamase = A.filter {
        it != 1
    }

    // Mai trebuie sa alegem doar 3 elemente
    val rezultat =
        combinari(
            elementeRamase.size.toLong(),
            3
        )

    println(
        "Numarul submultimilor este: $rezultat"
    )
}

2.

# Importam biblioteca tkinter
# Ea ne permite sa cream ferestre, butoane, meniuri etc.
import tkinter as tk

# messagebox -> pentru ferestre de dialog
# colorchooser -> pentru alegerea unei culori
from tkinter import messagebox, colorchooser


# =====================================================
# FUNCTIA APELATA CAND APASAM MOUSE-UL PE CANVAS
# =====================================================
def start_draw(event):

    # Variabile globale deoarece le folosim si
    # in alta functie (draw_line)
    global start_x, start_y

    # Salvam pozitia unde am apasat mouse-ul
    start_x = event.x
    start_y = event.y

    # Cand tinem apasat click-ul si miscam mouse-ul,
    # se va apela functia draw_line
    canvas.bind(
        "<B1-Motion>",
        lambda event: draw_line(event)
    )


# =====================================================
# FUNCTIA CARE DESENEAZA LINII
# =====================================================
def draw_line(event):

    global start_x, start_y

    # Desenam o linie intre:
    # punctul anterior
    # si noua pozitie a mouse-ului
    canvas.create_line(
        start_x,
        start_y,
        event.x,
        event.y,
        fill=current_color,
        width=2
    )

    # Schimbam textul etichetei.
    # Afisam operatia facuta.
    operations_label.config(
        text=
        f"Drawing line from "
        f"({start_x},{start_y}) "
        f"to ({event.x},{event.y})"
    )

    # Noua pozitie devine punct de plecare
    # pentru urmatoarea linie.
    start_x = event.x
    start_y = event.y


# =====================================================
# FUNCTIA DE STERGERE A CANVASULUI
# =====================================================
def clear_canvas():

    # Sterge toate desenele
    canvas.delete("all")

    # Modifica eticheta
    operations_label.config(
        text="Canvas cleared"
    )


# =====================================================
# FUNCTIA DE ALEGERE A CULORII
# =====================================================
def choose_color():

    global current_color

    # Se deschide o fereastra
    # pentru alegerea unei culori
    color = colorchooser.askcolor()[1]

    # Daca utilizatorul a ales o culoare
    if color:

        # Salvam noua culoare
        current_color = color

        # Schimbam culoarea butonului
        color_button.config(
            bg=current_color
        )


# =====================================================
# FEREASTRA DE INFORMATII
# =====================================================
def show_about():

    messagebox.showinfo(
        "About",
        "This is a simple drawing app using Tkinter."
    )


# =====================================================
# CREAREA FERESTREI PRINCIPALE
# =====================================================

root = tk.Tk()

# Titlul ferestrei
root.title("Drawing App")


# =====================================================
# CULOAREA INITIALA
# =====================================================
current_color = "black"


# =====================================================
# CREAREA MENIULUI
# =====================================================

menu = tk.Menu(root)

# Atasam meniul ferestrei
root.config(menu=menu)


# =====================================================
# MENIUL FILE
# =====================================================

file_menu = tk.Menu(
    menu,
    tearoff=0
)

# Adaugam meniul in bara de meniu
menu.add_cascade(
    label="File",
    menu=file_menu
)

# Optiunea Clear
file_menu.add_command(
    label="Clear",
    command=lambda: clear_canvas()
)

# Linie de separare
file_menu.add_separator()

# Optiunea Exit
file_menu.add_command(
    label="Exit",
    command=root.quit
)


# =====================================================
# MENIUL HELP
# =====================================================

help_menu = tk.Menu(
    menu,
    tearoff=0
)

menu.add_cascade(
    label="Help",
    menu=help_menu
)

help_menu.add_command(
    label="About",
    command=lambda: show_about()
)


# =====================================================
# CREAREA CANVASULUI
# =====================================================

canvas = tk.Canvas(
    root,
    width=500,
    height=400,
    bg="white"
)

canvas.pack(
    fill=tk.BOTH,
    expand=True
)


# =====================================================
# ETICHETA IN CARE AFISAM OPERATIILE
# =====================================================

operations_label = tk.Label(
    root,
    text="Draw something on the canvas",
    bg="lightgrey"
)

operations_label.pack(
    fill=tk.X
)


# =====================================================
# BUTON PENTRU SCHIMBAREA CULORII
# =====================================================

color_button = tk.Button(
    root,
    text="Choose Color",
    command=lambda: choose_color(),
    bg=current_color
)

color_button.pack(
    pady=5
)


# =====================================================
# ASOCIEREA EVENIMENTELOR DE MOUSE
# =====================================================

# Cand apasam click stanga
canvas.bind(
    "<Button-1>",
    lambda event: start_draw(event)
)


# =====================================================
# PORNIREA APLICATIEI
# =====================================================

root.mainloop()





SV65

Kot95 – Utilizând Kotlin și OOP să se creeze un program care permite modelarea unei săli de curs cu tot cu operații pentru obiecte. Se vor folosi clase enum. Se va desena diagrama UML (clase și obiecte). Se va explica maniera de aplicare a principiilor SOLID.

Py378 – Să se scrie un program Python utilizând multiprocessing care distribuie într-o manieră ciclică câte un cuvânt dintr-un fișier către alte trei procese și fiecare din acestea afișează numele lui și cuvântul primit la consolă. Comunicarea între procese se va realiza utilizând cozi.



1.

// Enum = o clasa speciala care are valori fixe.
// Aici spunem ce tipuri de cursuri pot exista.
enum class TipCurs {
    PROGRAMARE,
    BAZE_DATE,
    RETELE,
    MATEMATICA
}


// Alt enum.
// Aici spunem ce nivel poate avea un curs.
enum class Nivel {
    INCEPATOR,
    MEDIU,
    AVANSAT
}


// Clasa Curs modeleaza un curs real.
// Un curs are titlu, tip, nivel si capacitate maxima.
class Curs(
    val titlu: String,
    val tip: TipCurs,
    val nivel: Nivel,
    val capacitateMaxima: Int
) {

    // Lista cu studentii inscrisi la curs.
    // Este private ca sa nu poata fi modificata direct din exterior.
    // O modificam doar prin metoda inscrieStudent().
    private val studenti = mutableListOf<String>()


    // Metoda care inscrie un student la curs.
    fun inscrieStudent(nume: String) {

        // Verificam daca mai exista locuri.
        if (studenti.size < capacitateMaxima) {

            // Daca mai sunt locuri, adaugam studentul in lista.
            studenti.add(nume)

            println("$nume s-a inscris la cursul $titlu")
        } else {

            // Daca lista este plina, nu mai adaugam studentul.
            println("Cursul $titlu este plin.")
        }
    }


    // Metoda care verifica daca un curs este plin.
    fun estePlin(): Boolean {
        return studenti.size == capacitateMaxima
    }


    // Metoda care afiseaza informatiile despre curs.
    fun afiseaza() {
        println("Titlu: $titlu")
        println("Tip: $tip")
        println("Nivel: $nivel")
        println("Studenti inscrisi: ${studenti.size}")
        println("Capacitate maxima: $capacitateMaxima")
        println("Este plin: ${estePlin()}")
    }
}


// Clasa SirCursuri modeleaza o colectie de cursuri.
// Practic, tine mai multe obiecte de tip Curs.
class SirCursuri {

    // Lista de cursuri.
    // Este mutabila pentru ca vrem sa adaugam si sa stergem cursuri.
    // Este private pentru incapsulare.
    private val cursuri = mutableListOf<Curs>()


    // Adauga un curs in lista.
    fun adaugaCurs(curs: Curs) {
        cursuri.add(curs)
    }


    // Sterge un curs dupa titlu.
    fun stergeCurs(titlu: String) {
        cursuri.removeIf { curs ->
            curs.titlu == titlu
        }
    }


    // Cauta un curs dupa titlu.
    // Returneaza Curs? pentru ca se poate sa gaseasca sau sa nu gaseasca.
    // ? inseamna ca poate returna si null.
    fun cautaCurs(titlu: String): Curs? {
        return cursuri.find { curs ->
            curs.titlu == titlu
        }
    }


    // Afiseaza toate cursurile din lista.
    fun afiseazaCursuri() {
        println("Lista cursurilor:")

        for (curs in cursuri) {
            println("----------------------")
            curs.afiseaza()
        }
    }
}


// Functia main este punctul de pornire al programului.
fun main() {

    // Cream obiectul care va tine toate cursurile.
    val sir = SirCursuri()


    // Cream primul curs.
    val curs1 = Curs(
        "Programare Kotlin",
        TipCurs.PROGRAMARE,
        Nivel.INCEPATOR,
        3
    )


    // Cream al doilea curs.
    val curs2 = Curs(
        "Baze de Date",
        TipCurs.BAZE_DATE,
        Nivel.MEDIU,
        2
    )


    // Adaugam cursurile in colectia de cursuri.
    sir.adaugaCurs(curs1)
    sir.adaugaCurs(curs2)


    // Inscriem studenti la primul curs.
    curs1.inscrieStudent("Ana")
    curs1.inscrieStudent("Maria")
    curs1.inscrieStudent("Ion")

    // Acesta nu va mai intra, pentru ca limita este 3.
    curs1.inscrieStudent("George")


    println()


    // Afisam toate cursurile.
    sir.afiseazaCursuri()


    println()


    // Cautam un curs dupa titlu.
    val cursGasit = sir.cautaCurs("Baze de Date")

    if (cursGasit != null) {
        println("Curs gasit:")
        cursGasit.afiseaza()
    } else {
        println("Cursul nu a fost gasit.")
    }


    println()


    // Stergem cursul Baze de Date.
    sir.stergeCurs("Baze de Date")

    println("Dupa stergere:")

    // Afisam din nou lista.
    sir.afiseazaCursuri()
}

2.

# Biblioteca pentru procese
import multiprocessing

# Biblioteca folosita doar pentru o mica pauza
import time


# Functia executata de fiecare proces copil
# Primeste:
# 1. numele procesului
# 2. coada din care va primi cuvinte
def worker(nume, coada):

    # Procesul sta intr-o bucla infinita
    # si asteapta sa primeasca cuvinte
    while True:

        # Scoatem un cuvant din coada
        # Daca nu exista nimic in coada,
        # procesul asteapta automat
        cuvant = coada.get()

        # None este semnalul de oprire
        # Daca primim None iesim din bucla
        if cuvant is None:
            break

        # Afisam procesul si cuvantul primit
        print(f"{nume} a primit cuvantul: {cuvant}")

        # Pauza doar pentru a vedea mai usor executia
        time.sleep(1)


def main():

    # Numarul de procese copil
    nr_procese = 3

    # Lista in care salvam procesele
    procese = []

    # Lista in care salvam cozile
    cozi = []

    # Cream cele 3 procese
    for i in range(nr_procese):

        # Cream o coada noua
        # Fiecare proces are propria lui coada
        coada = multiprocessing.Queue()

        # Salvam coada in lista
        cozi.append(coada)

        # Cream procesul
        p = multiprocessing.Process(

            # Functia care va fi executata
            target=worker,

            # Parametrii functiei worker
            # worker("Proces-1", coada)
            # worker("Proces-2", coada)
            # worker("Proces-3", coada)
            args=(f"Proces-{i + 1}", coada)
        )

        # Pornim procesul
        p.start()

        # Salvam procesul in lista
        procese.append(p)

    # Deschidem fisierul
    with open("cuvinte.txt", "r") as fisier:

        # Citim tot textul din fisier
        text = fisier.read()

    # Impartim textul in cuvinte
    words = text.split()

    # Distribuire ciclica a cuvintelor
    for i, word in enumerate(words):

        # Alegem procesul:
        # i=0 -> Proces-1
        # i=1 -> Proces-2
        # i=2 -> Proces-3
        # i=3 -> Proces-1
        # i=4 -> Proces-2
        # i=5 -> Proces-3
        pozitie = i % nr_procese

        # Punem cuvantul in coada procesului ales
        cozi[pozitie].put(word)

    # Oprim procesele
    # Trimitem cate un None fiecarui proces
    for coada in cozi:
        coada.put(None)

    # Asteptam terminarea tuturor proceselor
    for p in procese:
        p.join()


# Punctul de pornire al programului
if __name__ == "__main__":
    main()




SV34

Kot40 – Utilizând funcțiile pentru procesarea submulțimii din Kotlin se va prelua un fișier text (creat de programator cu câteva propoziții în el) și se vor șterge primele două caractere dacă cuvântul are minim patru caractere. Se va utiliza combinație cu lambda peste colecții pentru procesare.

Pyt342 – Să se scrie un program Python care să deseneze utilizând tkinter grafic nouă dintr-o secvență de calcul.....
în intervalul 50 ≤ n ≤ 20. Să se afișeze separat într-o casetă suma și valoarea contorului.


1.

 import java.io.File

// modifica un singur cuvant
fun modificaCuvant(cuvant: String): String {
    return if (cuvant.length > 4) {
        cuvant.drop(2)
    } else {
        cuvant
    }
}

// modifica o propozitie
fun modificaPropozitie(propozitie: String): String {

    val cuvinte = propozitie.split(" ")

    val cuvinteModificate = cuvinte.map { cuvant ->
        modificaCuvant(cuvant)
    }

    return cuvinteModificate.joinToString(" ")
}

// proceseaza toate propozitiile
fun proceseazaPropozitii(
    propozitii: List<String>
): List<String> {

    return propozitii.map { propozitie ->
        modificaPropozitie(propozitie)
    }
}

fun afiseazaLista(lista: List<String>) {
    for (linie in lista) {
        println(linie)
    }
}

fun main() {

    // citim toate liniile din fisier
    val propozitii =
        File("propozitii.txt").readLines()

    // procesam propozitiile
    val rezultat =
        proceseazaPropozitii(propozitii)

    // afisam rezultatul
    afiseazaLista(rezultat)
}

2.

import tkinter as tk


# Aceasta functie calculeaza valoarea lui S pentru un anumit n
# Formula este:
# S = n + suma de la i = 0 pana la n - 4
def calculeaza_s(n):

    # La inceput suma este 0
    suma = 0

    # range(n - 3) genereaza valorile:
    # 0, 1, 2, ..., n - 4
    #
    # Exemplu:
    # daca n = 10
    # range(10 - 3) = range(7)
    # adica: 0, 1, 2, 3, 4, 5, 6
    for i in range(n - 3):

        # Adunam i la suma
        # Este acelasi lucru cu:
        # suma = suma + i
        suma += i

    # Dupa ce am calculat suma,
    # adunam n la ea
    s = n + suma

    # Returnam valoarea finala a lui S
    return s


# Aceasta functie genereaza toate valorile lui S
# care respecta conditia:
# 20 < S < 50
def genereaza_valori():

    # Lista in care punem perechi de forma:
    # (n, S)
    valori = []

    # Incercam mai multe valori pentru n
    # De la 1 pana la 29
    for n in range(1, 30):

        # Calculam S pentru valoarea curenta a lui n
        s = calculeaza_s(n)

        # Verificam daca S este intre 20 si 50
        if 20 < s < 50:

            # Daca este bun, il punem in lista
            valori.append((n, s))

    # Returnam toate valorile bune
    return valori


# Aceasta functie deseneaza graficul
def deseneaza_grafic(canvas, valori):

    # Pozitia primei bare pe axa X
    x = 50

    # Parcurgem toate perechile (n, S)
    for n, s in valori:

        # In Tkinter, coordonata Y creste in jos.
        # De aceea, ca bara sa creasca in sus,
        # facem 300 - s * 4.
        #
        # s * 4 este folosit doar ca sa se vada mai inalt graficul.
        y = 300 - s * 4

        # Desenam o bara sub forma de dreptunghi
        canvas.create_rectangle(
            x,        # partea stanga
            y,        # partea de sus
            x + 40,   # partea dreapta
            300,      # partea de jos
            fill="blue"
        )

        # Scriem valoarea lui S deasupra barei
        canvas.create_text(
            x + 20,
            y - 10,
            text=str(s)
        )

        # Scriem valoarea lui n sub bara
        canvas.create_text(
            x + 20,
            320,
            text=f"n={n}"
        )

        # Mutam urmatoarea bara mai la dreapta
        x += 60


# Aceasta functie afiseaza rezultatele intr-o fereastra separata
def afiseaza_rezultate(root, valori):

    # Cream o fereastra noua peste cea principala
    fereastra = tk.Toplevel(root)

    # Titlul ferestrei
    fereastra.title("Rezultate")

    # Textul in care vom pune toate rezultatele
    text = ""

    # Parcurgem valorile calculate
    for n, s in valori:

        # Adaugam cate o linie pentru fiecare rezultat
        text += f"n = {n}, S = {s}\n"

    # Cream o eticheta care afiseaza textul
    label = tk.Label(
        fereastra,
        text=text
    )

    # Punem eticheta in fereastra
    label.pack(padx=20, pady=20)


# Functia principala
def main():

    # Calculam valorile lui S care sunt intre 20 si 50
    valori = genereaza_valori()

    # Cream fereastra principala
    root = tk.Tk()

    # Punem titlul ferestrei
    root.title("Graficul lui S")

    # Cream zona alba pe care desenam graficul
    canvas = tk.Canvas(
        root,
        width=700,
        height=350,
        bg="white"
    )

    # Afisam canvas-ul in fereastra
    canvas.pack()

    # Desenam graficul pe baza valorilor calculate
    deseneaza_grafic(canvas, valori)

    # Afisam separat rezultatele
    afiseaza_rezultate(root, valori)

    # Tinem fereastra deschisa
    root.mainloop()


# Aceasta linie porneste programul
if __name__ == "__main__":
    main()




SV46

Kot 59 – Pornind de la două mulțimi A și B care conțin 20 de elemente depuse în două colecții separate și ținând cont de

A×B={(a,b)∣a∈A∧b∈B}

să se scrie un program Kotlin care va calcula

(A×B)∩(B×A)

utilizând funcții specifice colecțiilor și eventual lambda calculul următoare. Rezultatul este depus într-un dicționar iar acesta va fi afișat.

Pyt 339 – Utilizând modelul strategie să se creeze un program Python care să trateze diferențiat un mesaj de eroare în funcție de clasa lui (2-warning – scriu în fișier de warnings, 1-error – afișez la consolă și scriu în fișier de erori comune, 0-critical error – voi scrie în fișierul de erori grave și voi opri programul). Se vor utiliza excepții personalizate create de programator. Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


1.


fun produsCartezien(
    multime1: Set<Int>,
    multime2: Set<Int>
): Set<Pair<Int, Int>> {

    val rezultat = mutableSetOf<Pair<Int, Int>>()

    for (x in multime1) {
        for (y in multime2) {
            rezultat.add(Pair(x, y))
        }
    }

    return rezultat
}


fun intersectie(
    m1: Set<Pair<Int, Int>>,
    m2: Set<Pair<Int, Int>>
): Set<Pair<Int, Int>> {

    return m1.intersect(m2)
}


fun transformaInMap(
    perechi: Set<Pair<Int, Int>>
): Map<Int, Int> {

    val dictionar = mutableMapOf<Int, Int>()

    for (p in perechi) {
        dictionar[p.first] = p.second
    }

    return dictionar
}


fun afiseazaMap(
    dictionar: Map<Int, Int>
) {
    println("Rezultatul este:")

    for ((cheie, valoare) in dictionar) {
        println("$cheie -> $valoare")
    }
}


fun main() {

    val A = setOf(
        1,2,3,4,5,
        6,7,8,9,10,
        11,12,13,14,15,
        16,17,18,19,20
    )

    val B = setOf(
        10,11,12,13,14,
        15,16,17,18,19,
        20,21,22,23,24,
        25,26,27,28,29
    )

    val AxB = produsCartezien(A, B)

    val BxA = produsCartezien(B, A)

    val rezultat = intersectie(AxB, BxA)

    val dictionar = transformaInMap(rezultat)

    afiseazaMap(dictionar)
}


2.


# Exceptie personalizata pentru warning
class WarningPersonalizat(Exception):
    pass


# Exceptie personalizata pentru error
class ErrorPersonalizat(Exception):
    pass


# Exceptie personalizata pentru critical error
class CriticalErrorPersonalizat(Exception):
    pass


# Clasa de baza pentru strategii
class StrategieEroare:
    def trateaza(self, mesaj):
        raise NotImplementedError("Metoda trebuie implementata in clasele copil")


# Strategia pentru warning
class StrategieWarning(StrategieEroare):
    def trateaza(self, mesaj):
        with open("warnings.txt", "a") as fisier:
            fisier.write("WARNING: " + mesaj + "\n")


# Strategia pentru error normal
class StrategieError(StrategieEroare):
    def trateaza(self, mesaj):
        print("ERROR:", mesaj)

        with open("erori_comune.txt", "a") as fisier:
            fisier.write("ERROR: " + mesaj + "\n")


# Strategia pentru critical error
class StrategieCritical(StrategieEroare):
    def trateaza(self, mesaj):
        with open("erori_grave.txt", "a") as fisier:
            fisier.write("CRITICAL ERROR: " + mesaj + "\n")

        raise CriticalErrorPersonalizat("Programul s-a oprit din cauza unei erori grave.")


# Clasa care foloseste strategia potrivita
class ProcesatorEroare:
    def __init__(self):
        self.strategii = {
            2: StrategieWarning(),
            1: StrategieError(),
            0: StrategieCritical()
        }

    def proceseaza(self, clasa_eroare, mesaj):
        if clasa_eroare not in self.strategii:
            print("Clasa de eroare nu exista.")
            return

        strategie = self.strategii[clasa_eroare]
        strategie.trateaza(mesaj)


def main():
    procesator = ProcesatorEroare()

    erori = [
        (2, "Spatiu aproape plin pe disc"),
        (1, "Fisierul nu a fost gasit"),
        (0, "Eroare grava de sistem"),
        (2, "Memorie aproape plina")
    ]

    try:
        for clasa, mesaj in erori:
            procesator.proceseaza(clasa, mesaj)

    except CriticalErrorPersonalizat as e:
        print(e)


if __name__ == "__main__":
    main()



SV32

Kot 38 – Utilizând Kotlin să se aplice un functor peste elementele unui hashmap care va realiza următoarele procesări: va aplica f(x)=3x-1, ca lambda le va transforma în string și va afișa rezultatul.

Pyt356 – Utilizând funcții din itertools – Python și un contor tip closure să se genereze automat nume de fișiere temporare care vor fi create astfel s1-index-s2.tmp, unde s1 și s2 sunt nume primite la apelul metodei. Metoda va primi verificare unde a rămas generatorul și va întoarce următorul nume.



1.

fun transformaHashMap(
    hashMap: HashMap<Int, Int>,
    functie: (Int) -> Int
): List<String> {

    val rezultate = mutableListOf<String>()

    for ((cheie, valoare) in hashMap) {
        val rezultat = functie(valoare)
        val text = "Cheia $cheie -> f($valoare) = $rezultat"
        rezultate.add(text)
    }

    return rezultate
}

fun afiseazaRezultate(rezultate: List<String>) {
    for (r in rezultate) {
        println(r)
    }
}

fun main() {

    val hashMap = HashMap<Int, Int>()

    hashMap[1] = 2
    hashMap[2] = 5
    hashMap[3] = 10
    hashMap[4] = 7

    val functieLambda = { x: Int ->
        3 * x - 1
    }

    val rezultate = transformaHashMap(
        hashMap,
        functieLambda
    )

    afiseazaRezultate(rezultate)
}

2.

from itertools import count


# functie care creeaza un generator de nume de fisiere
def generator_fisiere(s1, s2):

    # contorul va genera: 1, 2, 3, 4, ...
    contor = count(1)

    # closure
    def urmatorul_fisier():

        # luam urmatorul numar din contor
        index = next(contor)

        # construim numele fisierului
        return f"{s1}-{index}-{s2}.tmp"

    return urmatorul_fisier


def main():
    # cream generatorul
    gen = generator_fisiere("abc", "test")

    # apeluri succesive
    print(gen())
    print(gen())
    print(gen())
    print(gen())
    print(gen())


if __name__ == "__main__":
    main()



SV39

Kot53 – Fie o mulțime la intrare A de 100 de elemente pare alese aleator care sunt depuse într-o colecție. Să se scrie un program în care va calcula
bn​=a12+a22+⋯+an2,∀n∈N∗
pentru orice valoare n în respectivul interval.

Pyt347 – Utilizând tkinter să se creeze un program Python care permite introducerea datelor unui angajat în companie (companie, departament, nume, data de naștere, funcție și salariu). Pentru introducerea datei de naștere se va folosi un combobox pentru selecția lunii. Programul va avea meniu.


1.

package com.pp.laborator

import kotlin.random.Random

fun main() {

    // ===================================================
    // CREAREA MULTIMII A
    // ===================================================
    // A va contine 100 de numere pare alese aleator.
    val A = mutableListOf<Int>()

    // Cat timp nu avem 100 de elemente,
    // continuam sa generam numere.
    while (A.size < 100) {

        // Generam un numar aleator intre 0 si 999.
        var nr = Random.nextInt(0, 1000)

        // Daca numarul este impar,
        // generam altul.
        while (nr % 2 != 0) {
            nr = Random.nextInt(0, 1000)
        }

        // Daca am ajuns aici,
        // inseamna ca numarul este par.
        // Il adaugam in colectie.
        A.add(nr)
    }

    // Afisam colectia generata.
    println("Multimea A:")
    println(A)

    // ===================================================
    // CALCULUL LUI bn
    // ===================================================
    //
    // Formula:
    //
    // bn = a1² + a2² + ... + an²
    //
    // Variabila bn va retine aceasta suma.
    var bn = 0

    // Parcurgem fiecare element din colectia A.
    for (x in A) {

        // Ridicam elementul la patrat:
        //
        // x² = x * x
        //
        // si il adunam la suma.
        bn = bn + x * x
    }

    // Afisam rezultatul final.
    println()
    println("bn = $bn")
}

2.

# Importam toate componentele de baza din tkinter.
# De aici folosim: Tk, Label, Entry, Button, Menu.
from tkinter import *

# Importam ttk pentru Combobox.
# Combobox = lista derulanta.
from tkinter import ttk

# Importam messagebox pentru a afisa ferestre cu mesaje.
from tkinter import messagebox


# Functia salveaza se executa cand apasam butonul Save.
def salveaza():

    # Construim un text in care punem toate datele introduse de utilizator.

    # entry_companie.get() citeste textul scris in campul Companie.
    text = "Companie: " + entry_companie.get() + "\n"

    # Adaugam departamentul.
    text += "Departament: " + entry_departament.get() + "\n"

    # Adaugam numele.
    text += "Nume: " + entry_nume.get() + "\n"

    # Adaugam data nasterii.
    # Ziua vine din Entry.
    # Luna vine din Combobox.
    # Anul vine din Entry.
    text += "Data nasterii: "
    text += entry_zi.get() + " "
    text += combo_luna.get() + " "
    text += entry_an.get() + "\n"

    # Adaugam functia.
    text += "Functie: " + entry_functie.get() + "\n"

    # Adaugam salariul.
    text += "Salariu: " + entry_salariu.get()

    # Afisam datele intr-o fereastra.
    messagebox.showinfo("Date angajat", text)


# Cream fereastra principala.
fereastra = Tk()

# Setam titlul ferestrei.
fereastra.title("Angajat")

# Setam dimensiunea ferestrei.
fereastra.geometry("450x350")


# =========================
# MENIU
# =========================

# Cream meniul principal.
meniu = Menu(fereastra)

# Atasam meniul la fereastra.
fereastra.config(menu=meniu)

# Cream submeniul Fisier.
# tearoff=0 inseamna ca meniul nu poate fi separat de fereastra.
fisier = Menu(meniu, tearoff=0)

# Punem submeniul Fisier in meniul principal.
meniu.add_cascade(label="Fisier", menu=fisier)

# Adaugam optiunea Iesire.
# Cand apasam Iesire, se inchide fereastra.
fisier.add_command(label="Iesire", command=fereastra.destroy)


# =========================
# CAMPUL COMPANIE
# =========================

# Label afiseaza textul "Companie".
# grid(row=0, column=0) pune textul pe randul 0, coloana 0.
Label(fereastra, text="Companie").grid(row=0, column=0)

# Entry creeaza un camp in care utilizatorul poate scrie.
entry_companie = Entry(fereastra)

# Punem campul pe randul 0, coloana 1.
entry_companie.grid(row=0, column=1)


# =========================
# CAMPUL DEPARTAMENT
# =========================

Label(fereastra, text="Departament").grid(row=1, column=0)

entry_departament = Entry(fereastra)

entry_departament.grid(row=1, column=1)


# =========================
# CAMPUL NUME
# =========================

Label(fereastra, text="Nume").grid(row=2, column=0)

entry_nume = Entry(fereastra)

entry_nume.grid(row=2, column=1)


# =========================
# DATA NASTERII
# =========================

# Ziua
Label(fereastra, text="Zi").grid(row=3, column=0)

# width=5 face campul mai mic.
entry_zi = Entry(fereastra, width=5)

entry_zi.grid(row=3, column=1)


# Luna
Label(fereastra, text="Luna").grid(row=3, column=2)

# Combobox = lista derulanta.
# Utilizatorul alege luna din lista.
combo_luna = ttk.Combobox(
    fereastra,
    values=[
        "Ianuarie",
        "Februarie",
        "Martie",
        "Aprilie",
        "Mai",
        "Iunie",
        "Iulie",
        "August",
        "Septembrie",
        "Octombrie",
        "Noiembrie",
        "Decembrie"
    ]
)

# Punem Combobox-ul pe randul 3, coloana 3.
combo_luna.grid(row=3, column=3)

# Alegem implicit prima valoare din lista, adica Ianuarie.
combo_luna.current(0)


# Anul
Label(fereastra, text="An").grid(row=3, column=4)

entry_an = Entry(fereastra, width=7)

entry_an.grid(row=3, column=5)


# =========================
# CAMPUL FUNCTIE
# =========================

Label(fereastra, text="Functie").grid(row=4, column=0)

entry_functie = Entry(fereastra)

entry_functie.grid(row=4, column=1)


# =========================
# CAMPUL SALARIU
# =========================

Label(fereastra, text="Salariu").grid(row=5, column=0)

entry_salariu = Entry(fereastra)

entry_salariu.grid(row=5, column=1)


# =========================
# BUTONUL SAVE
# =========================

# Button creeaza un buton.
# text="Save" este textul de pe buton.
# command=salveaza inseamna ca la apasare se executa functia salveaza.
Button(
    fereastra,
    text="Save",
    command=salveaza
).grid(
    row=6,
    column=0,
    columnspan=2
)


# mainloop porneste aplicatia.
# Fara mainloop, fereastra se inchide imediat.
fereastra.mainloop()




SV47

Kot 60 – Pornind de la două mulțimi A și B care conțin 20 de elemente depuse în două colecții separate și ținând cont de

A×B={(a,b)∣a∈A∧b∈B}

să se scrie un program Kotlin care va calcula

(A×B)∪(B×A)

utilizând funcții specifice colecțiilor și eventual lambda calculul următoare. Rezultatul este depus într-un dicționar iar acesta va fi afișat.

Pyth338 – Cu ajutorul modelului mediator să se creeze un program Python care va permite aplicarea succesivă a unor funcții asupra unui obiect de tip polinom și va reține obiectele la starea anterioară în momentul în care acest lucru este cerut (consolă):f1(x), dacă x par atunci x=x+1f2(x)=3x2−x2+x+1f3(x)=x+y
Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.
Notă: Ultima funcție este puțin neclară din cauza calității imaginii; textul pare să fie f3

(x)=x+y. Dacă ai o poză mai clară, pot verifica și corecta exact formula.



1.

package com.pp.laborator

// ===============================
// GENERAREA CELOR DOUA COLECTII
// ===============================

// List(20) creeaza o lista cu 20 de elemente.
//
// it ia valorile:
// 0,1,2,...,19
//
// it+1 produce:
// 1,2,3,...,20

val A = List(20) { it + 1 }

val B = List(20) { it + 1 }


// ===============================
// PRODUSUL CARTEZIAN
// ===============================
//
// A x B = toate perechile (a,b)
// unde:
// a apartine lui A
// b apartine lui B
//
// Exemplu:
//
// A={1,2}
// B={10,20}
//
// A x B =
// (1,10)
// (1,20)
// (2,10)
// (2,20)
//

fun cartezian(
    A: List<Int>,
    B: List<Int>
): Set<Pair<Int, Int>> {

    // multimea rezultat
    val rezultat =
        mutableSetOf<Pair<Int, Int>>()

    // luam fiecare element din A
    for (a in A) {

        // luam fiecare element din B
        for (b in B) {

            // cream perechea (a,b)
            val pereche =
                Pair(a, b)

            // o adaugam in multime
            rezultat.add(pereche)
        }
    }

    // returnam produsul cartezian
    return rezultat
}


// ===============================
// MAIN
// ===============================
fun main() {

    println("Incepe programul")

    // calculam A x B
    val prodAxB =
        cartezian(A, B)

    // calculam B x A
    val prodBxA =
        cartezian(B, A)

    // facem reuniunea:
    // (A x B) U (B x A)
    val reuniune =
        prodAxB.union(prodBxA)

    // depunem rezultatul intr-un dictionar
    //
    // cheia:
    // primul element din pereche
    //
    // valoarea:
    // al doilea element din pereche
    val dictionar =
        reuniune.groupBy(
            { it.first },
            { it.second }
        )

    println()

    println("Rezultatul:")

    println(dictionar)
}

2.

# ==========================================================
# MODELUL MEMENTO
# ==========================================================
# Problema:
# Avem o lista de numere.
# Aplicam pe lista functii precum f1, f2, f3.
# Inainte de modificare salvam lista.
# Daca vrem, putem reveni la o stare anterioara cu undo.


# ==========================================================
# CLASA MEMENTO
# ==========================================================
# Memento = obiectul care salveaza o stare veche.
# In cazul nostru, starea veche este o lista.
class Memento:

    def __init__(self, lista):
        # Constructorul primeste lista curenta.
        # Facem copy(), ca sa salvam o copie separata.
        # Altfel am salva aceeasi lista din memorie.
        self.lista = lista.copy()

    def getState(self):
        # Returneaza lista salvata.
        # Returnam tot copy(), ca sa nu modificam direct starea salvata.
        return self.lista.copy()


# ==========================================================
# CLASA ORIGINATOR
# ==========================================================
# Originator = obiectul care are lista curenta.
# El poate modifica lista si poate reveni la o stare veche.
class Originator:

    def __init__(self, lista):
        # Salvam lista primita in obiect.
        # Aceasta este starea curenta.
        self.lista = lista

    def f1(self):
        # f1:
        # daca x este par, atunci x devine x + 1
        # daca x este impar, ramane neschimbat.
        #
        # Exemplu:
        # [1, 2, 3, 4]
        # devine
        # [1, 3, 3, 5]

        functie = lambda x: x + 1 if x % 2 == 0 else x

        # Aplicam functia pe fiecare element din lista.
        self.lista = [functie(x) for x in self.lista]

    def f2(self):
        # f2(x) = 3*x^2 - 2*x + 1
        #
        # Exemplu pentru x = 3:
        # 3*3*3 - 2*3 + 1 = 22

        functie = lambda x: 3 * x * x - 2 * x + 1

        # Aplicam formula pe fiecare element.
        self.lista = [functie(x) for x in self.lista]

    def f3(self, i):
        # f3(x, y) = x + y
        #
        # In lista, alegem pozitia i si facem:
        # lista[i] = lista[i] + lista[i + 1]
        #
        # Exemplu:
        # lista = [10, 20, 30, 40]
        # f3(1)
        # lista[1] = 20 + 30 = 50
        # lista devine [10, 50, 30, 40]

        i = int(i)

        # Verificam ca indexul i sa fie valid
        # si sa existe si elementul urmator i + 1.
        if 0 <= i < len(self.lista) - 1:
            self.lista[i] = self.lista[i] + self.lista[i + 1]
        else:
            print("Index invalid sau nu exista element urmator.")

    def save(self):
        # Creeaza un Memento cu lista curenta.
        # Practic face un "save".
        return Memento(self.lista)

    def restore(self, memento):
        # Restaureaza lista dintr-un Memento.
        # Practic face un "load".
        self.lista = memento.getState()

    def getState(self):
        # Returneaza lista curenta, pentru afisare.
        return self.lista


# ==========================================================
# CLASA CARETAKER
# ==========================================================
# Caretaker = obiectul care tine istoricul salvarilor.
# El nu modifica lista direct.
class Caretaker:

    def __init__(self, originator):
        # Retinem originatorul.
        # Originatorul este obiectul care are lista.
        self.originator = originator

        # history este lista de stari salvate.
        # Aici punem obiecte Memento.
        self.history = []

    def save_state(self):
        # Salvam starea curenta a originatorului.
        # originator.save() returneaza un Memento.
        # append il adauga in istoric.
        self.history.append(self.originator.save())

    def undo(self):
        # Daca exista stari salvate:
        if self.history:

            # pop() scoate ultima stare salvata.
            memento = self.history.pop()

            # Restauram acea stare in originator.
            self.originator.restore(memento)

        else:
            print("Nu exista stare anterioara.")


# ==========================================================
# MAIN
# ==========================================================
if __name__ == "__main__":

    # Lista initiala.
    lista = [1, 2, 3, 4]

    # Cream originatorul.
    # El contine lista si o modifica.
    originator = Originator(lista)

    # Cream caretakerul.
    # El salveaza starile originatorului.
    caretaker = Caretaker(originator)

    print("Lista initiala:")
    print(originator.getState())

    # Salvam starea initiala:
    # history = [[1, 2, 3, 4]]
    caretaker.save_state()

    # Aplicam f1.
    # [1, 2, 3, 4] devine [1, 3, 3, 5]
    originator.f1()
    print("Dupa f1:")
    print(originator.getState())

    # Salvam starea de dupa f1:
    # history = [[1, 2, 3, 4], [1, 3, 3, 5]]
    caretaker.save_state()

    # Aplicam f2.
    # [1, 3, 3, 5] devine [2, 22, 22, 66]
    originator.f2()
    print("Dupa f2:")
    print(originator.getState())

    # Salvam starea de dupa f2:
    # history = [[1, 2, 3, 4], [1, 3, 3, 5], [2, 22, 22, 66]]
    caretaker.save_state()

    # Aplicam f3 pe pozitia 1.
    # [2, 22, 22, 66]
    # lista[1] = lista[1] + lista[2]
    # lista[1] = 22 + 22 = 44
    # lista devine [2, 44, 22, 66]
    originator.f3(1)
    print("Dupa f3(1):")
    print(originator.getState())

    # Undo:
    # scoate ultima stare salvata, adica [2, 22, 22, 66]
    # si revine la ea.
    caretaker.undo()
    print("Dupa primul undo:")
    print(originator.getState())

    # Undo din nou:
    # scoate [1, 3, 3, 5]
    # si revine la ea.
    caretaker.undo()
    print("Dupa al doilea undo:")
    print(originator.getState())

    # Undo din nou:
    # scoate [1, 2, 3, 4]
    # si revine la starea initiala.
    caretaker.undo()
    print("Dupa al treilea undo:")
    print(originator.getState())





SV53

Kot65 - Fie două colecții inițializate cu 100 de numere din submulțimile A={x∈N|x=(8n-18)/(2n-9),n∈N} și B={x∈Z|x=(9n²-48n+16)/(3n-8),n∈N}. Pentru acestea se vor calcula, utilizând funcții de transformare specifice și lambda calcul, următoarele operații: (A×B)∪(B∩A), unde A×B={(a,b)|a∈A∧b∈B}. Rezultatul este depus într-un hashmap iar acesta va fi afișat.

Py366 - Să se scrie un program Python utilizând threading, care procesează simultan, utilizând mai multe fire de execuție, un hashmap X și un dicționar Y cu formula f(x,y)=(x*y+y(i+1)), iar rezultatul este depus în Y. Dicționarul are pereche de valoare și index. Se va utiliza rlock().


1.

import kotlin.random.Random

// Aceasta functie creeaza multimea A.
// Parametrul "size" spune cate elemente vrem in multime.
// In enunt se cere 100, deci in main vom apela generateCollectionA(100).
fun generateCollectionA(size: Int): Set<Int> {

    // Cream o multime modificabila de Int.
    // "mutableSetOf" inseamna ca putem adauga elemente in ea.
    // "Set" nu permite duplicate.
    // Daca adaugam de doua ori aceeasi valoare, ea apare o singura data.
    val collection = mutableSetOf<Int>()

    // Cream un generator de numere aleatoare.
    // Il folosim ca sa alegem valori pentru n.
    val random = Random.Default

    // Repetam cat timp multimea NU are inca 100 de elemente.
    // collection.size = cate elemente sunt deja in multime.
    while (collection.size < size) {

        // Alegem un n natural aleator intre 1 si 999.
        // Limita 1000 nu este inclusa.
        // n reprezinta variabila din formula matematica.
        val n = random.nextInt(1, 1000)

        // Formula pentru A este:
        // x = (8n - 18) / (2n - 9)
        //
        // Aici calculam partea de sus a fractiei:
        // 8n - 18
        val numarator = 8 * n - 18

        // Aici calculam partea de jos a fractiei:
        // 2n - 9
        val numitor = 2 * n - 9

        // Nu avem voie sa impartim la 0.
        // De aceea verificam numitorul.
        if (numitor != 0) {

            // Calculam valoarea lui x.
            // Pentru ca lucram cu Int, impartirea este intreaga.
            // Exemplu: 7 / 2 devine 3, nu 3.5.
            val x = numarator / numitor

            // Adaugam valoarea calculata in multime.
            // Daca x exista deja, Set-ul nu il mai adauga a doua oara.
            collection.add(x)
        }
    }

    // Returnam multimea A obtinuta.
    return collection
}


// Aceasta functie creeaza multimea B.
// Si aici vrem tot 100 de elemente.
fun generateCollectionB(size: Int): Set<Int> {

    // Cream multimea B, modificabila.
    val collection = mutableSetOf<Int>()

    // Generator de numere aleatoare pentru n.
    val random = Random.Default

    // Repetam pana cand B are 100 de elemente diferite.
    while (collection.size < size) {

        // Alegem n natural aleator.
        val n = random.nextInt(1, 1000)

        // Formula pentru B este:
        // x = (9n^2 - 48n + 16) / (3n - 8)
        //
        // n^2 in Kotlin se scrie n * n.
        val numarator = 9 * n * n - 48 * n + 16

        // Calculam numitorul:
        // 3n - 8
        val numitor = 3 * n - 8

        // Verificam sa nu impartim la 0.
        if (numitor != 0) {

            // Calculam valoarea x pentru multimea B.
            val x = numarator / numitor

            // Adaugam valoarea in multime.
            collection.add(x)
        }
    }

    // Returnam multimea B.
    return collection
}


// Aceasta functie calculeaza produsul cartezian A x B.
//
// Produs cartezian inseamna:
// luam fiecare element a din A
// si il combinam cu fiecare element b din B.
//
// Exemplu:
// A = {1, 2}
// B = {10, 20}
//
// A x B =
// (1, 10)
// (1, 20)
// (2, 10)
// (2, 20)
fun produsCartezian(
    setA: Set<Int>,
    setB: Set<Int>
): Set<Pair<Int, Int>> {

    // Cream o multime in care vom pune perechile.
    // Pair<Int, Int> inseamna o pereche de doua numere intregi.
    val rezultat = mutableSetOf<Pair<Int, Int>>()

    // Primul for ia pe rand fiecare element din A.
    for (a in setA) {

        // Al doilea for ia pe rand fiecare element din B.
        for (b in setB) {

            // Cream perechea (a, b).
            // Pair(a, b) inseamna exact (a,b) din matematica.
            val pereche = Pair(a, b)

            // Adaugam perechea in rezultat.
            rezultat.add(pereche)
        }
    }

    // Returnam toate perechile obtinute.
    return rezultat
}


// Aceasta functie calculeaza intersectia dintre doua multimi.
// Intersectia inseamna elementele comune.
//
// Exemplu:
// A = {1, 2, 3}
// B = {2, 3, 5}
//
// A ∩ B = {2, 3}
fun intersectie(
    setA: Set<Int>,
    setB: Set<Int>
): Set<Int> {

    // intersect este functie existenta in Kotlin.
    // Ea returneaza elementele care exista si in setA, si in setB.
    return setA.intersect(setB)
}


fun main() {

    // Enuntul cere doua colectii cu 100 de numere.
    val size = 100

    // Apelam functia care genereaza multimea A.
    // Rezultatul este salvat in variabila A.
    val A = generateCollectionA(size)

    // Apelam functia care genereaza multimea B.
    // Rezultatul este salvat in variabila B.
    val B = generateCollectionB(size)

    // Afisam A ca sa vedem ce valori s-au generat.
    println("Multimea A:")
    println(A)
    println()

    // Afisam B.
    println("Multimea B:")
    println(B)
    println()

    // Calculam A x B.
    // Adica toate perechile (a,b),
    // unde a este din A si b este din B.
    val axb = produsCartezian(A, B)

    // Calculam B ∩ A.
    // Este acelasi lucru cu A ∩ B,
    // adica valorile comune dintre cele doua multimi.
    val intersectia = intersectie(B, A)

    // Enuntul cere ca rezultatul sa fie depus intr-un HashMap.
    //
    // HashMap<Int, Any> inseamna:
    // cheia este Int: 0, 1, 2, 3...
    // valoarea este Any.
    //
    // Folosim Any deoarece in HashMap punem:
    // 1. perechi de tip Pair<Int, Int>
    // 2. numere simple de tip Int
    //
    // Any permite sa punem ambele tipuri.
    val rezultat = HashMap<Int, Any>()

    // index va fi cheia din HashMap.
    // Prima valoare va avea cheia 0,
    // a doua cheia 1,
    // a treia cheia 2 etc.
    var index = 0

    // Punem in HashMap toate perechile din A x B.
    for (pereche in axb) {

        // rezultat[index] = pereche inseamna:
        // la cheia index salvez perechea curenta.
        rezultat[index] = pereche

        // Crestem indexul pentru urmatoarea valoare.
        index++
    }

    // Punem in HashMap si elementele din intersectie.
    for (x in intersectia) {

        // La cheia index salvam numarul x.
        rezultat[index] = x

        // Crestem indexul.
        index++
    }

    // Afisam rezultatul final.
    println("Rezultatul final din HashMap:")

    // forEach parcurge fiecare pereche cheie-valoare din HashMap.
    rezultat.forEach { (cheie, valoare) ->

        // Afisam cheia si valoarea.
        println("$cheie -> $valoare")
    }
}

2. 

import threading

# X este dictionarul / hashmap-ul cu valorile x.
# Cheia este indexul i.
# Valoarea este x-ul folosit in formula.
X = {
    0: 1,
    1: 2,
    2: 3,
    3: 4
}

# Y este dictionarul in care avem valorile y.
# Cheia este indexul i.
# Valoarea este y[i].
Y = {
    0: 10,
    1: 20,
    2: 30,
    3: 40
}

# Cream un RLock.
# RLock este folosit pentru sincronizare intre thread-uri.
# Cu "with lock", doar un thread poate modifica Y la un moment dat.
lock = threading.RLock()


# Aceasta functie este executata de fiecare thread.
# Fiecare thread primeste un index i.
# Pentru acel i, se calculeaza:
# Y[i] = X[i] * Y[i] + Y[i + 1]
def formula(X, Y, i):

    # Intram in zona protejata de lock.
    # Asta inseamna ca doar un thread executa codul de aici la un moment dat.
    with lock:

        # Verificam daca exista Y[i + 1].
        # Pentru ultimul element, Y[i + 1] nu exista.
        #
        # Exemplu:
        # daca Y are cheile 0,1,2,3
        # pentru i = 3 nu exista Y[4].
        if i < len(Y) - 1:

            # Aplicam formula din enunt:
            # f(x,y) = x * y_i + y_{i+1}
            #
            # In cod:
            # x = X[i]
            # y_i = Y[i]
            # y_{i+1} = Y[i + 1]
            #
            # Rezultatul se pune inapoi in Y[i].
            Y[i] = X[i] * Y[i] + Y[i + 1]

        else:
            # Pentru ultimul element nu putem aplica formula,
            # deoarece nu exista Y[i + 1].
            # Il lasam neschimbat.
            Y[i] = Y[i]

        # Afisam ce a calculat thread-ul curent.
        print(
            threading.current_thread().name,
            "a calculat Y[", i, "] =",
            Y[i]
        )


# Aceasta functie creeaza si porneste thread-urile.
def run(X, Y):

    # Lista in care pastram thread-urile create.
    threads = []

    # Parcurgem indicii dictionarului Y.
    # Daca Y are 4 elemente, range(len(Y)) inseamna:
    # 0, 1, 2, 3.
    for i in range(len(Y)):

        # Cream un thread.
        # target=formula inseamna ca thread-ul va executa functia formula.
        # args=(X, Y, i) sunt parametrii trimisi functiei formula.
        t = threading.Thread(
            target=formula,
            args=(X, Y, i)
        )

        # Adaugam thread-ul in lista,
        # ca sa il putem astepta mai tarziu cu join().
        threads.append(t)

        # Pornim thread-ul.
        # Din acest moment, thread-ul incepe sa execute formula(X, Y, i).
        t.start()

    # Dupa ce am pornit toate thread-urile,
    # le asteptam sa se termine.
    for t in threads:

        # join() inseamna:
        # programul principal asteapta terminarea thread-ului t.
        t.join()


# Programul principal.
if __name__ == "__main__":

    # Afisam Y inainte de procesare.
    print("Y initial:")
    print(Y)

    # Pornim procesarea pe mai multe thread-uri.
    run(X, Y)

    print()

    # Afisam Y dupa ce toate thread-urile au terminat.
    print("Y final:")
    print(Y)




SV107

Kot100 – Să se scrie un program Kotlin care va crea echivalentul unei loterii 6/49, o va utiliza într-un exemplu simplu de utilizare a unui hashmap reținut pe mai multe fire de execuție și va permite extragerea acelor bile câștigătoare.

Pyth323 – Utilizând modelul proxy să se creeze în Python o aplicație care permite introducerea de obiecte de tip pacient cu nume și prenume și numărul de boală. Vom avea o fereastră de tip textbox (acesta este doar un exemplu) și un buton. La apăsarea acestuia se va crea o fereastră de tip combobox unde se vor putea alege medicii și bolnavii de Covid. Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


1. 

package com.pp.laborator

import java.util.HashMap

// Clasa care implementeaza o bariera.
// Niciun fir nu trece mai departe pana nu ajung toate firele.
class Bariera(private val nrMaxFire: Int) {

    // Obiect folosit pentru sincronizare.
    // Pe el vom face synchronized, wait si notifyAll.
    private val lock = Object()

    // Numarul de fire care au ajuns la bariera.
    private var nrFire = 0

    // Functia apelata de fiecare fir cand ajunge la bariera.
    fun await() = synchronized(lock) {

        // Firul curent a ajuns la bariera.
        nrFire++

        println("${Thread.currentThread().name} a ajuns la bariera.")

        // Daca nu au ajuns toate firele,
        // firul curent se blocheaza.
        while (nrFire < nrMaxFire) {
            lock.wait()
        }

        // Ultimul fir care ajunge la bariera
        // trezeste toate firele blocate.
        lock.notifyAll()
    }
}


// Clasa care reprezinta un fir de executie.
// Fiecare fir calculeaza o suma partiala din HashMap.
class FirSuma(
    private val nume: String,
    private val hashMap: HashMap<Int, Int>,
    private val start: Int,
    private val stop: Int,
    private val bariera: Bariera
) : Thread(nume) {

    // Aici salvam suma calculata de fir.
    var sumaPartiala = 0

    // Codul executat de fir.
    override fun run() {

        // Parcurgem cheile de la start la stop.
        for (i in start..stop) {

            // Adaugam valoarea din HashMap.
            // Daca cheia nu exista, adaugam 0.
            sumaPartiala += hashMap[i] ?: 0
        }

        println("$nume a calculat suma partiala = $sumaPartiala")

        // Firul asteapta celelalte fire la bariera.
        bariera.await()

        // Aici ajunge doar dupa ce toate firele
        // au trecut de bariera.
        println("$nume a trecut de bariera.")
    }
}


fun main() {

    println("Inceputul programului")

    // Cream HashMap-ul.
    val hashMap = HashMap<Int, Int>()

    // Introducem perechi cheie-valoare.
    hashMap[1] = 10
    hashMap[2] = 20
    hashMap[3] = 30
    hashMap[4] = 40
    hashMap[5] = 50
    hashMap[6] = 60

    // Cream o bariera pentru 3 fire.
    val bariera = Bariera(3)

    // Primul fir calculeaza:
    // 10 + 20 = 30
    val t1 = FirSuma(
        "Thread1",
        hashMap,
        1,
        2,
        bariera
    )

    // Al doilea fir calculeaza:
    // 30 + 40 = 70
    val t2 = FirSuma(
        "Thread2",
        hashMap,
        3,
        4,
        bariera
    )

    // Al treilea fir calculeaza:
    // 50 + 60 = 110
    val t3 = FirSuma(
        "Thread3",
        hashMap,
        5,
        6,
        bariera
    )

    // Pornim firele.
    // Fiecare fir va executa metoda run().
    t1.start()
    t2.start()
    t3.start()

    // Firul principal asteapta terminarea lor.
    t1.join()
    t2.join()
    t3.join()

    // Adunam sumele partiale.
    val sumaFinala =
        t1.sumaPartiala +
        t2.sumaPartiala +
        t3.sumaPartiala

    println("Suma finala = $sumaFinala")
} 

2.
# RecordsMemory este clasa care se ocupa doar cu salvarea in fisier.
# Ea nu stie nimic despre boombox, video, radio etc.
class RecordsMemory:

    # variabila clasei
    # aici tinem numele fisierului unde salvam mesajele
    fisier = "memory.txt"

    # metoda save primeste un mesaj si il scrie in fisier
    def save(self, mesaj):

        # deschidem fisierul in modul "a"
        # "a" vine de la append, adica adaugare la final
        # daca fisierul nu exista, Python il creeaza automat
        f = open(self.fisier, "a")

        # scriem mesajul primit in fisier
        # "\n" inseamna linie noua
        f.write(mesaj + "\n")

        # inchidem fisierul dupa ce am terminat de scris
        f.close()

        # afisam mesaj pe ecran
        print("Am salvat in memorie")


# Sound este clasa care se ocupa doar cu volumul
class Sound:

    # constructorul primeste volumul maxim permis
    def __init__(self, maxSound):

        # salvam volumul maxim
        self.maxSound = maxSound

        # volumul curent este initial 0
        self.volum = 0

    # metoda pentru modificarea volumului
    def set_volume(self, volum):

        # verificam daca volumul cerut este mai mic sau egal cu maximul permis
        if volum <= self.maxSound:

            # daca este corect, il salvam
            self.volum = volum

            # afisam volumul curent
            print("Volumul este", self.volum)

        else:
            # daca volumul este prea mare, nu il modificam
            print("Volumul este prea mare")


# Video se ocupa de comenzile de redare
class Video:

    # porneste redarea
    def play(self):
        print("Pornesc melodia")

    # deruleaza inainte
    def fast_forward(self):
        print("Derulez inainte")

    # deruleaza inapoi
    def rewind(self):
        print("Derulez inapoi")


# Radio se ocupa doar de radio
class Radio:

    # porneste radioul
    def start(self):
        print("Radio pornit")


# Battery se ocupa doar de baterie
class Battery:

    # afiseaza statusul bateriei
    def status(self):
        print("Bateria este la 80%")


# Recorder se ocupa doar de inregistrare
class Recorder:

    # porneste inregistrarea
    def record(self):
        print("Inregistrez")


# Boombox este clasa FATADE.
# Ea ascunde toate clasele din spate:
# Video, Radio, Battery, Recorder, Sound, RecordsMemory.
class Boombox:

    # constructorul creeaza toate componentele interne
    def __init__(self):

        # obiect pentru redare
        self.video = Video()

        # obiect pentru radio
        self.radio = Radio()

        # obiect pentru baterie
        self.battery = Battery()

        # obiect pentru inregistrare
        self.recorder = Recorder()

        # obiect pentru volum
        # 10 este volumul maxim permis
        self.sound = Sound(10)

        # obiect pentru salvarea in fisier
        self.memory = RecordsMemory()

    # metoda simpla oferita clientului
    # clientul scrie box.play()
    # dar in spate se apeleaza video.play()
    def play(self):
        self.video.play()

    # metoda simpla pentru derulare inainte
    def fast_forward(self):
        self.video.fast_forward()

    # metoda simpla pentru derulare inapoi
    def rewind(self):
        self.video.rewind()

    # metoda simpla pentru radio
    def radio_on(self):
        self.radio.start()

    # metoda simpla pentru inregistrare
    def record(self):

        # pornim inregistrarea
        self.recorder.record()

        # salvam in fisier faptul ca s-a facut o inregistrare
        self.memory.save("S-a realizat o inregistrare.")

    # metoda simpla pentru volum
    def volume(self, volum):
        self.sound.set_volume(volum)

    # metoda simpla pentru baterie
    def battery_status(self):
        self.battery.status()


# main-ul programului
if __name__ == "__main__":

    print("Programul incepe")

    # cream un singur obiect Boombox
    # acesta este fatada
    box = Boombox()

    # clientul foloseste doar obiectul box
    # nu creeaza direct Video, Radio, Battery etc.

    box.play()

    box.fast_forward()

    box.rewind()

    box.radio_on()

    box.record()

    box.volume(5)

    box.battery_status()
