```
SV30

Kot34 - Utilizând să se scrie un program Kotlin care preia un fișier text (scris de programator) la intrare, organizează cuvintele într-un arbore binar utilizând o constrângere la alegere, iar apoi cu ajutorul unei clase specifice și al funcțiilor va afișa porțiuni din arbore.

Pyth361 - Să se creeze o aplicație Python pentru rezervarea unei camere de hostel. Camerele (cu atributele și operatorii lor) sunt obiecte și sunt reținute într-un arbore. Cu ajutorul unui decorator implementați OOP peste funcțiile de bază și se vor implementa un flux suplimentar de tratare al plății următorului serviciu suplimentar: consum din barul camerei când se generează nota de plată. Se vor prezenta și diagramele de clase și obiecte. Se va explica maniera de aplicare a principiilor SOLID.

kotlin 34

import java.io.File

class Nod(
    val cuvant: String
) {
    var stanga: Nod? = null
    var dreapta: Nod? = null
}


class Arbore {

    var radacina: Nod? = null

    fun adauga(cuvant: String) {

        val nodNou = Nod(cuvant)

        if (radacina == null) {
            radacina = nodNou
            return
        }

        var curent = radacina

        while (true) {

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
                return
            }
        }
    }

    fun afisare(nod: Nod?) {

        if (nod == null)
            return

        afisare(nod.stanga)

        println(nod.cuvant)

        afisare(nod.dreapta)
    }

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


fun main() {

    val arbore = Arbore()

    val text =
        File("text.txt").readText()

    val cuvinte =
        text.split(" ")

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

class Camera:

    def __init__(self, numar, tip, pret):

        self.numar = numar
        self.tip = tip
        self.pret = pret

        self.consum_bar = 0


class ADTCamere:

    def __init__(self):

        self.camere = []

    def adauga_camera(self, camera):

        self.camere.append(camera)

    def cauta_camera(self, numar):

        for camera in self.camere:

            if camera.numar == numar:
                return camera

        return None

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


def decorator_consum_bar(functie):

    def wrapper(rezervare):

        total = functie(rezervare)

        total = (
                total +
                rezervare.camera.consum_bar
        )

        return total

    return wrapper


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


def main():

    hotel = ADTCamere()

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

    hotel.adauga_camera(camera1)
    hotel.adauga_camera(camera2)
    hotel.adauga_camera(camera3)

    print("Camere disponibile:")
    hotel.afiseaza_camere()

    print()

    camera_aleasa = hotel.cauta_camera(102)

    if camera_aleasa is not None:

        camera_aleasa.consum_bar = 75

        rezervare = Rezervare(
            "Ion Popescu",
            camera_aleasa,
            3
        )

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

    val transformare: (Int) -> Int = { x ->
        x + 3
    }

    launch {

        lista.indices

            .filter { it % 2 == 1 }

            .forEach {

                lista[it] = transformare(lista[it])
            }

    }.join()

    println()
    println("Lista finala:")
    println(lista)
}

python 066

from abc import ABC, abstractmethod


class Mediator(ABC):

    @abstractmethod
    def trimite_mesaj(self, mesaj, expeditor):
        pass


class ChatMusuroi(Mediator):

    def __init__(self):
        self.furnici = []

    def adauga_furnica(self, furnica):
        self.furnici.append(furnica)

    def trimite_mesaj(self, mesaj, expeditor):

        print()
        print(f"{expeditor.nume} trimite mesajul: {mesaj}")

        for furnica in self.furnici:

            if furnica != expeditor:
                furnica.primeste_mesaj(mesaj, expeditor)


class Furnica:

    def __init__(self, nume, mediator):
        self.nume = nume
        self.mediator = mediator

        self.mediator.adauga_furnica(self)

    def spune(self, mesaj):
        self.mediator.trimite_mesaj(mesaj, self)

    def primeste_mesaj(self, mesaj, expeditor):
        print(
            f"{self.nume} a primit de la "
            f"{expeditor.nume}: {mesaj}"
        )

    def __str__(self):
        return self.nume


def main():

    chat = ChatMusuroi()

    furnica1 = Furnica("Furnica Mica", chat)
    furnica2 = Furnica("Furni", chat)
    furnica3 = Furnica("Furnicuta", chat)
    furnica4 = Furnica("Soldat", chat)

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

import java.io.File

fun main() {


    val text = File("text.txt").readText()

    println("Textul din fisier:")
    println(text)
    println()


    val cuvinte = text.split(" ")

    println("Cuvintele sunt:")
    println(cuvinte)
    println()


    val rezultat = cuvinte

        .filter {
            it.length >= 4
        }

        .map {

            val mijloc = it.length / 2

            it.substring(
                mijloc - 1,
                mijloc + 1
            )
        }


    println("Cele doua caractere din mijloc:")

    rezultat.forEach {

        println(it)
    }
}

2.

class MediatorFurnici:

    def __init__(self):

        self.furnici = []

    def adauga_furnica(self, furnica):

        self.furnici.append(furnica)

    def trimite_mesaj(self, expeditor, mesaj):

        print()
        print(f"{expeditor.nume} trimite mesajul: {mesaj}")

        for furnica in self.furnici:

            if furnica != expeditor:

                furnica.primeste_mesaj(
                    expeditor,
                    mesaj
                )


class Furnica:

    def __init__(self, nume, mediator):

        self.nume = nume

        self.mediator = mediator

        self.mediator.adauga_furnica(self)

    def trimite_mesaj(self, mesaj):

        self.mediator.trimite_mesaj(
            self,
            mesaj
        )

    def primeste_mesaj(self, expeditor, mesaj):

        print(
            f"{self.nume} a primit de la "
            f"{expeditor.nume}: {mesaj}"
        )


def main():

    mediator = MediatorFurnici()

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

    furnica1.trimite_mesaj("Am gasit mancare!")

    furnica2.trimite_mesaj("Drum bun spre musuroi.")

    furnica3.trimite_mesaj("Pericol in apropiere!")

    furnica4.trimite_mesaj("Am nevoie de ajutor!")


if __name__ == "__main__":
    main()


SV80

Kot70 – Utilizând modelul adaptor să se creeze un program Kotlin care va primi la intrare diverse tipuri de date simple sau de tip colecție și va realiza afișarea lor ca o singură operație care primește chiar obiectul. Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.

Pyt81 – Utilizând modelul command să se scrie în Python un program care pornind de la o clasă student va asigura posibilitatea unei comenzi specifice unui obiect profesor care să conducă la schimbarea stării interne a unui obiect student (de ex. din fericit în disperat). Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


1.

interface Target {

    fun afiseaza(obiect: Any?)
}


class AfisareAdapter : Target {

    override fun afiseaza(obiect: Any?) {

        when (obiect) {

            is String -> {
                println("String: $obiect")
            }

            is Int -> {
                println("Int: $obiect")
            }

            is Double -> {
                println("Double: $obiect")
            }

            is Boolean -> {
                println("Boolean: $obiect")
            }

            is List<*> -> {
                println("Lista:")
                for (element in obiect) {
                    println(element)
                }
            }

            is Set<*> -> {
                println("Set:")
                for (element in obiect) {
                    println(element)
                }
            }

            is Map<*, *> -> {
                println("Map:")
                for ((cheie, valoare) in obiect) {
                    println("$cheie -> $valoare")
                }
            }

            null -> {
                println("Obiect null")
            }

            else -> {
                println("Tip de date necunoscut: $obiect")
            }
        }
    }
}


class Client(
    private val adapter: Target
) {

    fun proceseaza(obiect: Any?) {
        adapter.afiseaza(obiect)
    }
}


fun main() {

    val adapter = AfisareAdapter()

    val client = Client(adapter)

    client.proceseaza("Salut Kotlin")
    client.proceseaza(100)
    client.proceseaza(25.5)
    client.proceseaza(true)

    println()

    val lista = listOf("Ana", "Maria", "Ion")
    client.proceseaza(lista)

    println()

    val set = setOf(1, 2, 3, 4)
    client.proceseaza(set)

    println()

    val dictionar = mapOf(
        "Ana" to 10,
        "Ion" to 8,
        "Maria" to 9
    )

    client.proceseaza(dictionar)
}

2.

class Student:

    def __init__(self, nume):

        self.nume = nume

        self.stare = "fericit"

    def schimba_stare(self, stare_noua):

        self.stare = stare_noua

    def afiseaza_stare(self):

        print(
            f"Studentul {self.nume} este {self.stare}."
        )


class Comanda:

    def executa(self):
        pass


class ComandaDisperat(Comanda):

    def __init__(self, student):

        self.student = student

    def executa(self):

        self.student.schimba_stare("disperat")


class ComandaFericit(Comanda):

    def __init__(self, student):

        self.student = student

    def executa(self):

        self.student.schimba_stare("fericit")


class ComandaObosit(Comanda):

    def __init__(self, student):

        self.student = student

    def executa(self):

        self.student.schimba_stare("obosit")


class Profesor:

    def __init__(self, nume):

        self.nume = nume

    def trimite_comanda(self, comanda):

        print(f"Profesorul {self.nume} trimite o comanda.")

        comanda.executa()


def main():

    student = Student("Ion")

    student.afiseaza_stare()

    print()

    profesor = Profesor("Popescu")

    comanda_disperat = ComandaDisperat(student)
    comanda_fericit = ComandaFericit(student)
    comanda_obosit = ComandaObosit(student)

    profesor.trimite_comanda(comanda_disperat)
    student.afiseaza_stare()

    print()

    profesor.trimite_comanda(comanda_obosit)
    student.afiseaza_stare()

    print()

    profesor.trimite_comanda(comanda_fericit)
    student.afiseaza_stare()


if __name__ == "__main__":
    main()


SV50

Kot 62 – Pornind de la două mulțimi A și B care conțin 20 de elemente depuse în două colecții separate și ținând cont de A×B={(a,b)∣a∈A∧b∈B}, să se scrie un program Kotlin care va calcula {Ai reunit cu Bi
} utilizând funcții specifice colecțiilor și eventual lambda calculului următoare. Rezultatul este depus într-un dicționar care apoi va fi afișat.

Pyth335 – Utilizând modelul observator să se creeze în Python pentru un obiect de tip recalculare a prețului, în funcție de niște rate de reducere introduse extern de la tastatură (prin intermediul unui model proxy cu validare pe user-parolă), un logger (scriere automată în jurnal a operațiilor) care va scrie într-un fișier separat user și data la care a fost efectuată modificarea de preț și ce modificare a fost realizată. Se vor desena diagrame de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


1.

fun genereazaMultime(): Set<Int> {

    val multime = mutableSetOf<Int>()

    while (multime.size < 20) {
        multime.add((1..50).random())
    }

    return multime
}


fun produsCartezian(
    prima: Set<Int>,
    aDoua: Set<Int>
): List<Pair<Int, Int>> {

    val rezultat = mutableListOf<Pair<Int, Int>>()

    for (x in prima) {

        for (y in aDoua) {

            rezultat.add(Pair(x, y))
        }
    }

    return rezultat
}


fun afiseazaPerechi(perechi: List<Pair<Int, Int>>) {

    for (pereche in perechi) {
        println("(${pereche.first}, ${pereche.second})")
    }
}


fun main() {

    val A = genereazaMultime()
    val B = genereazaMultime()

    println("Multimea A:")
    println(A)

    println()

    println("Multimea B:")
    println(B)

    println()

    val reuniune = A.union(B)

    val intersectie = B.intersect(A)

    println("A ∪ B:")
    println(reuniune)

    println()

    println("B ∩ A:")
    println(intersectie)

    println()

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


class Observer:

    def update(self, mesaj):
        pass


class Logger(Observer):

    def update(self, mesaj):

        with open("log.txt", "a") as fisier:
            fisier.write(mesaj + "\n")

        print("LOG:", mesaj)


class Laptop:

    def __init__(self, nume, pret):

        self.nume = nume
        self.pret = pret

        self.observers = []

    def attach(self, observer):

        self.observers.append(observer)

    def notify(self, mesaj):

        for observer in self.observers:
            observer.update(mesaj)

    def modifica_pret(self, pret_nou):

        self.pret = pret_nou

        mesaj = (
            f"Pretul laptopului "
            f"{self.nume} "
            f"a devenit "
            f"{self.pret} lei."
        )

        self.notify(mesaj)


class LaptopProxy:

    def __init__(self, laptop):

        self.laptop = laptop

    def modifica_pret(
            self,
            parola,
            pret_nou
    ):

        if parola == "1234":

            self.laptop.modifica_pret(
                pret_nou
            )

        else:
            print("Parola gresita!")


def main():

    laptop = Laptop(
        "Asus",
        3500
    )

    logger = Logger()

    laptop.attach(logger)

    proxy = LaptopProxy(laptop)

    print()

    proxy.modifica_pret(
        "1234",
        4000
    )

    print()

    proxy.modifica_pret(
        "1111",
        5000
    )

    print()

    proxy.modifica_pret(
        "1234",
        4200
    )


if __name__ == "__main__":
    main()


SV44

Kot57 – Să se scrie un program Kotlin care, având la intrare două mulțimi A și B de câte 15 numere inițializate aleator care vor fi depuse în niște colecții, să se aplice utilizând calculul lambda și operatorii specifici următoarea transformare:

A×B={(a,b)∣a∈A∧b∈B}

iar rezultatul trebuie la rândul lui depus într-o colecție și aceasta afișată.

Pyth359 – Să se creeze o aplicație Python pentru rezervarea unei camere de hostel. Camerele (cu atributele și operatorii lor) sunt obiecte și sunt reținute într-un arbore. Cu ajutorul unor decoratori peste funcțiile de bază se vor implementa un flux suplimentar de tratare a plății următorului serviciu suplimentar: consum din barul camerei când se generează nota de plată. Se vor prezenta și diagramele de clase și obiecte


1.

fun genereazaMultime(): List<Int> {

    return List(15) {
        (1..100).random()
    }
}


fun produsCartezian(
    A: List<Int>,
    B: List<Int>
): List<Pair<Int, Int>> {

    val rezultat = mutableListOf<Pair<Int, Int>>()

    for (a in A) {

        for (b in B) {

            val pereche = Pair(a, b)

            rezultat.add(pereche)
        }
    }

    return rezultat
}


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

    val A = genereazaMultime()
    val B = genereazaMultime()

    println("Multimea A:")
    println(A)

    println()

    println("Multimea B:")
    println(B)

    println()

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

from functools import wraps


class Camera:

    def __init__(self, numar, tip, pret):

        self.numar = numar

        self.tip = tip

        self.pret = pret

        self.bar_consum = 0

    def __str__(self):
        return (
            f"Camera {self.numar}, "
            f"tip {self.tip}, "
            f"pret/noapte {self.pret} RON"
        )


class ADTCamere:

    def __init__(self):

        self.camere = []

    def adauga_camera(self, camera):
        self.camere.append(camera)

    def cauta_camera(self, numar):

        for camera in self.camere:

            if camera.numar == numar:
                return camera

        return None

    def afiseaza_camere(self):

        for camera in self.camere:
            print(camera)


def cu_bar_decorator(functie):

    @wraps(functie)
    def wrapper(rezervare):

        pret_total = functie(rezervare)

        pret_total += rezervare.camera.bar_consum

        return pret_total

    return wrapper


class Rezervare:

    def __init__(
        self,
        camera,
        nume_client,
        zile
    ):

        self.camera = camera

        self.nume_client = nume_client

        self.zile = zile

    @cu_bar_decorator
    def calcul_pret_total(self):

        return (
            self.camera.pret *
            self.zile
        )

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


def main():

    hotel = ADTCamere()

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

    hotel.adauga_camera(camera1)
    hotel.adauga_camera(camera2)
    hotel.adauga_camera(camera3)

    print("Camere disponibile:")
    hotel.afiseaza_camere()

    print()

    camera_rezervata = hotel.cauta_camera(102)

    if camera_rezervata is not None:

        camera_rezervata.bar_consum = 75

        rezervare = Rezervare(
            camera_rezervata,
            "Ion Popescu",
            3
        )

        print("NOTA DE PLATA:")
        print(rezervare)

    else:
        print("Camera nu exista.")


if __name__ == "__main__":
    main()


SV38

Kot52 – Utilizând ADT-ul (ovenul lambda) și transformări specifice colecțiilor având la intrare mulțimea:

A={1,2,3,…,100}

să se scrie un program Kotlin care va determina numărul submulțimilor cu două elemente ale mulțimii A, care conțin cel puțin o componentă impară.

Pyth346 – Utilizând tkinter și funcții lambda să se creeze un program Python care va afișa în eticheta ferestrei operațiile realizate cu mouse-ul în momentul în care facem un desen simplu (de ex. trag un triunghi din linii) pe un canvas din acea fereastră. Programul va avea meniu.


1.

fun combinari(n: Long, k: Long): Long {

    var rezultat = 1L

    for (i in 1..k) {
        rezultat = rezultat * (n - i + 1) / i
    }

    return rezultat
}


fun main() {

    val A = (1..100).toList()

    val elementeRamase = A.filter {
        it != 1
    }

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

import tkinter as tk

from tkinter import messagebox, colorchooser


def start_draw(event):

    global start_x, start_y

    start_x = event.x
    start_y = event.y

    canvas.bind(
        "<B1-Motion>",
        lambda event: draw_line(event)
    )


def draw_line(event):

    global start_x, start_y

    canvas.create_line(
        start_x,
        start_y,
        event.x,
        event.y,
        fill=current_color,
        width=2
    )

    operations_label.config(
        text=
        f"Drawing line from "
        f"({start_x},{start_y}) "
        f"to ({event.x},{event.y})"
    )

    start_x = event.x
    start_y = event.y


def clear_canvas():

    canvas.delete("all")

    operations_label.config(
        text="Canvas cleared"
    )


def choose_color():

    global current_color

    color = colorchooser.askcolor()[1]

    if color:

        current_color = color

        color_button.config(
            bg=current_color
        )


def show_about():

    messagebox.showinfo(
        "About",
        "This is a simple drawing app using Tkinter."
    )


root = tk.Tk()

root.title("Drawing App")


current_color = "black"


menu = tk.Menu(root)

root.config(menu=menu)


file_menu = tk.Menu(
    menu,
    tearoff=0
)

menu.add_cascade(
    label="File",
    menu=file_menu
)

file_menu.add_command(
    label="Clear",
    command=lambda: clear_canvas()
)

file_menu.add_separator()

file_menu.add_command(
    label="Exit",
    command=root.quit
)


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


operations_label = tk.Label(
    root,
    text="Draw something on the canvas",
    bg="lightgrey"
)

operations_label.pack(
    fill=tk.X
)


color_button = tk.Button(
    root,
    text="Choose Color",
    command=lambda: choose_color(),
    bg=current_color
)

color_button.pack(
    pady=5
)


canvas.bind(
    "<Button-1>",
    lambda event: start_draw(event)
)


root.mainloop()


SV65

Kot95 – Utilizând Kotlin și OOP să se creeze un program care permite modelarea unei săli de curs cu tot cu operații pentru obiecte. Se vor folosi clase enum. Se va desena diagrama UML (clase și obiecte). Se va explica maniera de aplicare a principiilor SOLID.

Py378 – Să se scrie un program Python utilizând multiprocessing care distribuie într-o manieră ciclică câte un cuvânt dintr-un fișier către alte trei procese și fiecare din acestea afișează numele lui și cuvântul primit la consolă. Comunicarea între procese se va realiza utilizând cozi.


1.

enum class TipCurs {
    PROGRAMARE,
    BAZE_DATE,
    RETELE,
    MATEMATICA
}


enum class Nivel {
    INCEPATOR,
    MEDIU,
    AVANSAT
}


class Curs(
    val titlu: String,
    val tip: TipCurs,
    val nivel: Nivel,
    val capacitateMaxima: Int
) {

    private val studenti = mutableListOf<String>()


    fun inscrieStudent(nume: String) {

        if (studenti.size < capacitateMaxima) {

            studenti.add(nume)

            println("$nume s-a inscris la cursul $titlu")
        } else {

            println("Cursul $titlu este plin.")
        }
    }


    fun estePlin(): Boolean {
        return studenti.size == capacitateMaxima
    }


    fun afiseaza() {
        println("Titlu: $titlu")
        println("Tip: $tip")
        println("Nivel: $nivel")
        println("Studenti inscrisi: ${studenti.size}")
        println("Capacitate maxima: $capacitateMaxima")
        println("Este plin: ${estePlin()}")
    }
}


class SirCursuri {

    private val cursuri = mutableListOf<Curs>()


    fun adaugaCurs(curs: Curs) {
        cursuri.add(curs)
    }


    fun stergeCurs(titlu: String) {
        cursuri.removeIf { curs ->
            curs.titlu == titlu
        }
    }


    fun cautaCurs(titlu: String): Curs? {
        return cursuri.find { curs ->
            curs.titlu == titlu
        }
    }


    fun afiseazaCursuri() {
        println("Lista cursurilor:")

        for (curs in cursuri) {
            println("----------------------")
            curs.afiseaza()
        }
    }
}


fun main() {

    val sir = SirCursuri()


    val curs1 = Curs(
        "Programare Kotlin",
        TipCurs.PROGRAMARE,
        Nivel.INCEPATOR,
        3
    )


    val curs2 = Curs(
        "Baze de Date",
        TipCurs.BAZE_DATE,
        Nivel.MEDIU,
        2
    )


    sir.adaugaCurs(curs1)
    sir.adaugaCurs(curs2)


    curs1.inscrieStudent("Ana")
    curs1.inscrieStudent("Maria")
    curs1.inscrieStudent("Ion")

    curs1.inscrieStudent("George")


    println()


    sir.afiseazaCursuri()


    println()


    val cursGasit = sir.cautaCurs("Baze de Date")

    if (cursGasit != null) {
        println("Curs gasit:")
        cursGasit.afiseaza()
    } else {
        println("Cursul nu a fost gasit.")
    }


    println()


    sir.stergeCurs("Baze de Date")

    println("Dupa stergere:")

    sir.afiseazaCursuri()
}

2.

import multiprocessing

import time


def worker(nume, coada):

    while True:

        cuvant = coada.get()

        if cuvant is None:
            break

        print(f"{nume} a primit cuvantul: {cuvant}")

        time.sleep(1)


def main():

    nr_procese = 3

    procese = []

    cozi = []

    for i in range(nr_procese):

        coada = multiprocessing.Queue()

        cozi.append(coada)

        p = multiprocessing.Process(

            target=worker,

            args=(f"Proces-{i + 1}", coada)
        )

        p.start()

        procese.append(p)

    with open("cuvinte.txt", "r") as fisier:

        text = fisier.read()

    words = text.split()

    for i, word in enumerate(words):

        pozitie = i % nr_procese

        cozi[pozitie].put(word)

    for coada in cozi:
        coada.put(None)

    for p in procese:
        p.join()


if __name__ == "__main__":
    main()


SV34

Kot40 – Utilizând funcțiile pentru procesarea submulțimii din Kotlin se va prelua un fișier text (creat de programator cu câteva propoziții în el) și se vor șterge primele două caractere dacă cuvântul are minim patru caractere. Se va utiliza combinație cu lambda peste colecții pentru procesare.

Pyt342 – Să se scrie un program Python care să deseneze utilizând tkinter grafic nouă dintr-o secvență de calcul.....
în intervalul 50 ≤ n ≤ 20. Să se afișeze separat într-o casetă suma și valoarea contorului.


1.

 import java.io.File

fun modificaCuvant(cuvant: String): String {
    return if (cuvant.length > 4) {
        cuvant.drop(2)
    } else {
        cuvant
    }
}

fun modificaPropozitie(propozitie: String): String {

    val cuvinte = propozitie.split(" ")

    val cuvinteModificate = cuvinte.map { cuvant ->
        modificaCuvant(cuvant)
    }

    return cuvinteModificate.joinToString(" ")
}

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

    val propozitii =
        File("propozitii.txt").readLines()

    val rezultat =
        proceseazaPropozitii(propozitii)

    afiseazaLista(rezultat)
}

2.

import tkinter as tk


def calculeaza_s(n):

    suma = 0

    for i in range(n - 3):

        suma += i

    s = n + suma

    return s


def genereaza_valori():

    valori = []

    for n in range(1, 30):

        s = calculeaza_s(n)

        if 20 < s < 50:

            valori.append((n, s))

    return valori


def deseneaza_grafic(canvas, valori):

    x = 50

    for n, s in valori:

        y = 300 - s * 4

        canvas.create_rectangle(
            x,
            y,
            x + 40,
            300,
            fill="blue"
        )

        canvas.create_text(
            x + 20,
            y - 10,
            text=str(s)
        )

        canvas.create_text(
            x + 20,
            320,
            text=f"n={n}"
        )

        x += 60


def afiseaza_rezultate(root, valori):

    fereastra = tk.Toplevel(root)

    fereastra.title("Rezultate")

    text = ""

    for n, s in valori:

        text += f"n = {n}, S = {s}\n"

    label = tk.Label(
        fereastra,
        text=text
    )

    label.pack(padx=20, pady=20)


def main():

    valori = genereaza_valori()

    root = tk.Tk()

    root.title("Graficul lui S")

    canvas = tk.Canvas(
        root,
        width=700,
        height=350,
        bg="white"
    )

    canvas.pack()

    deseneaza_grafic(canvas, valori)

    afiseaza_rezultate(root, valori)

    root.mainloop()


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


class WarningPersonalizat(Exception):
    pass


class ErrorPersonalizat(Exception):
    pass


class CriticalErrorPersonalizat(Exception):
    pass


class StrategieEroare:
    def trateaza(self, mesaj):
        raise NotImplementedError("Metoda trebuie implementata in clasele copil")


class StrategieWarning(StrategieEroare):
    def trateaza(self, mesaj):
        with open("warnings.txt", "a") as fisier:
            fisier.write("WARNING: " + mesaj + "\n")


class StrategieError(StrategieEroare):
    def trateaza(self, mesaj):
        print("ERROR:", mesaj)

        with open("erori_comune.txt", "a") as fisier:
            fisier.write("ERROR: " + mesaj + "\n")


class StrategieCritical(StrategieEroare):
    def trateaza(self, mesaj):
        with open("erori_grave.txt", "a") as fisier:
            fisier.write("CRITICAL ERROR: " + mesaj + "\n")

        raise CriticalErrorPersonalizat("Programul s-a oprit din cauza unei erori grave.")


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


def generator_fisiere(s1, s2):

    contor = count(1)

    def urmatorul_fisier():

        index = next(contor)

        return f"{s1}-{index}-{s2}.tmp"

    return urmatorul_fisier


def main():
    gen = generator_fisiere("abc", "test")

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

    val A = mutableListOf<Int>()

    while (A.size < 100) {

        var nr = Random.nextInt(0, 1000)

        while (nr % 2 != 0) {
            nr = Random.nextInt(0, 1000)
        }

        A.add(nr)
    }

    println("Multimea A:")
    println(A)

    var bn = 0

    for (x in A) {

        bn = bn + x * x
    }

    println()
    println("bn = $bn")
}

2.

from tkinter import *

from tkinter import ttk

from tkinter import messagebox


def salveaza():


    text = "Companie: " + entry_companie.get() + "\n"

    text += "Departament: " + entry_departament.get() + "\n"

    text += "Nume: " + entry_nume.get() + "\n"

    text += "Data nasterii: "
    text += entry_zi.get() + " "
    text += combo_luna.get() + " "
    text += entry_an.get() + "\n"

    text += "Functie: " + entry_functie.get() + "\n"

    text += "Salariu: " + entry_salariu.get()

    messagebox.showinfo("Date angajat", text)


fereastra = Tk()

fereastra.title("Angajat")

fereastra.geometry("450x350")


meniu = Menu(fereastra)

fereastra.config(menu=meniu)

fisier = Menu(meniu, tearoff=0)

meniu.add_cascade(label="Fisier", menu=fisier)

fisier.add_command(label="Iesire", command=fereastra.destroy)


Label(fereastra, text="Companie").grid(row=0, column=0)

entry_companie = Entry(fereastra)

entry_companie.grid(row=0, column=1)


Label(fereastra, text="Departament").grid(row=1, column=0)

entry_departament = Entry(fereastra)

entry_departament.grid(row=1, column=1)


Label(fereastra, text="Nume").grid(row=2, column=0)

entry_nume = Entry(fereastra)

entry_nume.grid(row=2, column=1)


Label(fereastra, text="Zi").grid(row=3, column=0)

entry_zi = Entry(fereastra, width=5)

entry_zi.grid(row=3, column=1)


Label(fereastra, text="Luna").grid(row=3, column=2)

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

combo_luna.grid(row=3, column=3)

combo_luna.current(0)


Label(fereastra, text="An").grid(row=3, column=4)

entry_an = Entry(fereastra, width=7)

entry_an.grid(row=3, column=5)


Label(fereastra, text="Functie").grid(row=4, column=0)

entry_functie = Entry(fereastra)

entry_functie.grid(row=4, column=1)


Label(fereastra, text="Salariu").grid(row=5, column=0)

entry_salariu = Entry(fereastra)

entry_salariu.grid(row=5, column=1)


Button(
    fereastra,
    text="Save",
    command=salveaza
).grid(
    row=6,
    column=0,
    columnspan=2
)


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


val A = List(20) { it + 1 }

val B = List(20) { it + 1 }


fun cartezian(
    A: List<Int>,
    B: List<Int>
): Set<Pair<Int, Int>> {

    val rezultat =
        mutableSetOf<Pair<Int, Int>>()

    for (a in A) {

        for (b in B) {

            val pereche =
                Pair(a, b)

            rezultat.add(pereche)
        }
    }

    return rezultat
}


fun main() {

    println("Incepe programul")

    val prodAxB =
        cartezian(A, B)

    val prodBxA =
        cartezian(B, A)

    val reuniune =
        prodAxB.union(prodBxA)

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


class Memento:

    def __init__(self, lista):
        self.lista = lista.copy()

    def getState(self):
        return self.lista.copy()


class Originator:

    def __init__(self, lista):
        self.lista = lista

    def f1(self):

        functie = lambda x: x + 1 if x % 2 == 0 else x

        self.lista = [functie(x) for x in self.lista]

    def f2(self):

        functie = lambda x: 3 * x * x - 2 * x + 1

        self.lista = [functie(x) for x in self.lista]

    def f3(self, i):

        i = int(i)

        if 0 <= i < len(self.lista) - 1:
            self.lista[i] = self.lista[i] + self.lista[i + 1]
        else:
            print("Index invalid sau nu exista element urmator.")

    def save(self):
        return Memento(self.lista)

    def restore(self, memento):
        self.lista = memento.getState()

    def getState(self):
        return self.lista


class Caretaker:

    def __init__(self, originator):
        self.originator = originator

        self.history = []

    def save_state(self):
        self.history.append(self.originator.save())

    def undo(self):
        if self.history:

            memento = self.history.pop()

            self.originator.restore(memento)

        else:
            print("Nu exista stare anterioara.")


if __name__ == "__main__":

    lista = [1, 2, 3, 4]

    originator = Originator(lista)

    caretaker = Caretaker(originator)

    print("Lista initiala:")
    print(originator.getState())

    caretaker.save_state()

    originator.f1()
    print("Dupa f1:")
    print(originator.getState())

    caretaker.save_state()

    originator.f2()
    print("Dupa f2:")
    print(originator.getState())

    caretaker.save_state()

    originator.f3(1)
    print("Dupa f3(1):")
    print(originator.getState())

    caretaker.undo()
    print("Dupa primul undo:")
    print(originator.getState())

    caretaker.undo()
    print("Dupa al doilea undo:")
    print(originator.getState())

    caretaker.undo()
    print("Dupa al treilea undo:")
    print(originator.getState())


SV53

Kot65 - Fie două colecții inițializate cu 100 de numere din submulțimile A={x∈N|x=(8n-18)/(2n-9),n∈N} și B={x∈Z|x=(9n²-48n+16)/(3n-8),n∈N}. Pentru acestea se vor calcula, utilizând funcții de transformare specifice și lambda calcul, următoarele operații: (A×B)∪(B∩A), unde A×B={(a,b)|a∈A∧b∈B}. Rezultatul este depus într-un hashmap iar acesta va fi afișat.

Py366 - Să se scrie un program Python utilizând threading, care procesează simultan, utilizând mai multe fire de execuție, un hashmap X și un dicționar Y cu formula f(x,y)=(x*y+y(i+1)), iar rezultatul este depus în Y. Dicționarul are pereche de valoare și index. Se va utiliza rlock().


1.

import kotlin.random.Random

fun generateCollectionA(size: Int): Set<Int> {

    val collection = mutableSetOf<Int>()

    val random = Random.Default

    while (collection.size < size) {

        val n = random.nextInt(1, 1000)

        val numarator = 8 * n - 18

        val numitor = 2 * n - 9

        if (numitor != 0) {

            val x = numarator / numitor

            collection.add(x)
        }
    }

    return collection
}


fun generateCollectionB(size: Int): Set<Int> {

    val collection = mutableSetOf<Int>()

    val random = Random.Default

    while (collection.size < size) {

        val n = random.nextInt(1, 1000)

        val numarator = 9 * n * n - 48 * n + 16

        val numitor = 3 * n - 8

        if (numitor != 0) {

            val x = numarator / numitor

            collection.add(x)
        }
    }

    return collection
}


fun produsCartezian(
    setA: Set<Int>,
    setB: Set<Int>
): Set<Pair<Int, Int>> {

    val rezultat = mutableSetOf<Pair<Int, Int>>()

    for (a in setA) {

        for (b in setB) {

            val pereche = Pair(a, b)

            rezultat.add(pereche)
        }
    }

    return rezultat
}


fun intersectie(
    setA: Set<Int>,
    setB: Set<Int>
): Set<Int> {

    return setA.intersect(setB)
}


fun main() {

    val size = 100

    val A = generateCollectionA(size)

    val B = generateCollectionB(size)

    println("Multimea A:")
    println(A)
    println()

    println("Multimea B:")
    println(B)
    println()

    val axb = produsCartezian(A, B)

    val intersectia = intersectie(B, A)

    val rezultat = HashMap<Int, Any>()

    var index = 0

    for (pereche in axb) {

        rezultat[index] = pereche

        index++
    }

    for (x in intersectia) {

        rezultat[index] = x

        index++
    }

    println("Rezultatul final din HashMap:")

    rezultat.forEach { (cheie, valoare) ->

        println("$cheie -> $valoare")
    }
}

2.

import threading

X = {
    0: 1,
    1: 2,
    2: 3,
    3: 4
}

Y = {
    0: 10,
    1: 20,
    2: 30,
    3: 40
}

lock = threading.RLock()


def formula(X, Y, i):

    with lock:

        if i < len(Y) - 1:

            Y[i] = X[i] * Y[i] + Y[i + 1]

        else:
            Y[i] = Y[i]

        print(
            threading.current_thread().name,
            "a calculat Y[", i, "] =",
            Y[i]
        )


def run(X, Y):

    threads = []

    for i in range(len(Y)):

        t = threading.Thread(
            target=formula,
            args=(X, Y, i)
        )

        threads.append(t)

        t.start()

    for t in threads:

        t.join()


if __name__ == "__main__":

    print("Y initial:")
    print(Y)

    run(X, Y)

    print()

    print("Y final:")
    print(Y)


SV107

Kot100 – Să se scrie un program Kotlin care va crea echivalentul unei loterii 6/49, o va utiliza într-un exemplu simplu de utilizare a unui hashmap reținut pe mai multe fire de execuție și va permite extragerea acelor bile câștigătoare.

Pyth323 – Utilizând modelul proxy să se creeze în Python o aplicație care permite introducerea de obiecte de tip pacient cu nume și prenume și numărul de boală. Vom avea o fereastră de tip textbox (acesta este doar un exemplu) și un buton. La apăsarea acestuia se va crea o fereastră de tip combobox unde se vor putea alege medicii și bolnavii de Covid. Se vor desena diagrama de clase și de obiecte. Se va explica maniera de aplicare a principiilor SOLID.


1.

package com.pp.laborator

import java.util.HashMap

class Bariera(private val nrMaxFire: Int) {

    private val lock = Object()

    private var nrFire = 0

    fun await() = synchronized(lock) {

        nrFire++

        println("${Thread.currentThread().name} a ajuns la bariera.")

        while (nrFire < nrMaxFire) {
            lock.wait()
        }

        lock.notifyAll()
    }
}


class FirSuma(
    private val nume: String,
    private val hashMap: HashMap<Int, Int>,
    private val start: Int,
    private val stop: Int,
    private val bariera: Bariera
) : Thread(nume) {

    var sumaPartiala = 0

    override fun run() {

        for (i in start..stop) {

            sumaPartiala += hashMap[i] ?: 0
        }

        println("$nume a calculat suma partiala = $sumaPartiala")

        bariera.await()

        println("$nume a trecut de bariera.")
    }
}


fun main() {

    println("Inceputul programului")

    val hashMap = HashMap<Int, Int>()

    hashMap[1] = 10
    hashMap[2] = 20
    hashMap[3] = 30
    hashMap[4] = 40
    hashMap[5] = 50
    hashMap[6] = 60

    val bariera = Bariera(3)

    val t1 = FirSuma(
        "Thread1",
        hashMap,
        1,
        2,
        bariera
    )

    val t2 = FirSuma(
        "Thread2",
        hashMap,
        3,
        4,
        bariera
    )

    val t3 = FirSuma(
        "Thread3",
        hashMap,
        5,
        6,
        bariera
    )

    t1.start()
    t2.start()
    t3.start()

    t1.join()
    t2.join()
    t3.join()

    val sumaFinala =
        t1.sumaPartiala +
        t2.sumaPartiala +
        t3.sumaPartiala

    println("Suma finala = $sumaFinala")
}

2.
class RecordsMemory:

    fisier = "memory.txt"

    def save(self, mesaj):

        f = open(self.fisier, "a")

        f.write(mesaj + "\n")

        f.close()

        print("Am salvat in memorie")


class Sound:

    def __init__(self, maxSound):

        self.maxSound = maxSound

        self.volum = 0

    def set_volume(self, volum):

        if volum <= self.maxSound:

            self.volum = volum

            print("Volumul este", self.volum)

        else:
            print("Volumul este prea mare")


class Video:

    def play(self):
        print("Pornesc melodia")

    def fast_forward(self):
        print("Derulez inainte")

    def rewind(self):
        print("Derulez inapoi")


class Radio:

    def start(self):
        print("Radio pornit")


class Battery:

    def status(self):
        print("Bateria este la 80%")


class Recorder:

    def record(self):
        print("Inregistrez")


class Boombox:

    def __init__(self):

        self.video = Video()

        self.radio = Radio()

        self.battery = Battery()

        self.recorder = Recorder()

        self.sound = Sound(10)

        self.memory = RecordsMemory()

    def play(self):
        self.video.play()

    def fast_forward(self):
        self.video.fast_forward()

    def rewind(self):
        self.video.rewind()

    def radio_on(self):
        self.radio.start()

    def record(self):

        self.recorder.record()

        self.memory.save("S-a realizat o inregistrare.")

    def volume(self, volum):
        self.sound.set_volume(volum)

    def battery_status(self):
        self.battery.status()


if __name__ == "__main__":

    print("Programul incepe")

    box = Boombox()


    box.play()

    box.fast_forward()

    box.rewind()

    box.radio_on()

    box.record()

    box.volume(5)

    box.battery_status()
```
