# Java_allatok
```
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeMap;

public class Main {

    private class Allat {
        public String fajta;
        public int magas;
        public int suly;
        public int kor;

        public Allat(String sor) {
            String[] s = sor.split(";");
            fajta = s[0];
            magas = Integer.parseInt(s[1]);
            suly = Integer.parseInt(s[2]);
            kor = Integer.parseInt(s[3]);
        }
    }

    private ArrayList<Allat> allatok = new ArrayList<>();

    public Main() {
        // 0. feladat
        betolt("allatok.csv");
        System.out.printf("0) Összesen %d féle állatfajta adata beolvasva\n", allatok.size());

        // 1. feladat
        Allat legMagasabb = allatok.get(0);
        for (Allat a : allatok) if (a.magas > legMagasabb.magas) legMagasabb = a;
        System.out.printf("1) A legmagasabb állatfajta: %s, %dcm\n", legMagasabb.fajta, legMagasabb.magas);

        // 2. feladat
        double korOsszeg = 0; int korDb = 0; for (Allat a : allatok) if (a.suly > 20) { korOsszeg += a.kor; korDb++; }
        System.out.printf("2) A húsz kilónál nehezebb fajták átlagéletkora: %.2f év\n", korOsszeg/korDb);

        // 3. feladat
        int sulyOsszeg = 0; for (Allat a : allatok) sulyOsszeg += a.suly;
        int sulyAtlag = sulyOsszeg / allatok.size();
        Allat atlagos = allatok.get(0);
        for (Allat a : allatok) if (Math.abs(a.suly-sulyAtlag) < Math.abs(atlagos.suly-sulyAtlag)) atlagos = a;
        System.out.printf("3) Az átlagsúlyhoz (%dkg) legközelebbi fajta: %s (%dkg)\n", sulyAtlag, atlagos.fajta, atlagos.suly);

        // 4. feladat
        System.out.printf("4) Kettős betű van a fajta nevében:\n");
        for (Allat a : allatok) {
            boolean dupla = false;
            for (int i=0; i<a.fajta.length()-1; i++) if (a.fajta.charAt(i) == a.fajta.charAt(i+1)) dupla = true;
            if (dupla) System.out.printf("   * %s\n", a.fajta);
        }

        // 5. feladat
        TreeMap<Integer, Integer> magasDb = new TreeMap<>();
        for (Allat a : allatok) {
            int kat = a.magas/50;
            if (!magasDb.containsKey(kat)) magasDb.put(kat, 1);
            else magasDb.put(kat, magasDb.get(kat)+1);
        }
        System.out.printf("5) Magasság kategórák (50cm):\n");
        ArrayList<Integer> magasKat = new ArrayList<>();
        for (Integer k : magasDb.keySet()) {
            magasKat.add(k);
            System.out.printf("   * %03d-%03dcm: %d darab\n", k*50, k*50+49, magasDb.get(k));
        }

        // 6. feladat
        int v = magasKat.get((int)(Math.random()*magasKat.size()));
        System.out.printf("6) Ebből egy véletlen kategóriába (%d-%dcm) eső állatok:\n   * ", v*50, v*50+49);
        String vesszo = "";
        for (Allat a : allatok) {
            if (a.magas >= v*50 && a.magas <= v*50+49) { System.out.printf(vesszo + "%s", a.fajta); vesszo = ", "; }
        }

        // 7. feladat
        PrintWriter ki = null;
        try {
            ki = new PrintWriter(new File("kicsi.csv"), "utf-8");
            for (Allat a : allatok) {
                if (a.magas < 100) ki.printf("%s;%d;%d;%d\r\n", a.fajta, a.magas, a.suly, a.kor);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (ki != null) ki.close();
        }
    }

    public void betolt(String fajlnev) {
        Scanner be = null;
        try {
            be = new Scanner(new File(fajlnev), "utf-8");
            be.nextLine();
            while (be.hasNextLine()) allatok.add(new Allat(be.nextLine()));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (be != null) be.close();
        }
    }

    public static void main(String[] args) {
	    new Main();
    }
}
```
### allatok.csv
```
Állatfaj;Magasság(cm);Súly(kg);Életkor(év)
Afrikai elefánt;330;6000;62
Indiai orrszarvú;180;2200;45
Zsiráf;500;800;28
Zebra;150;350;27
Tigris;110;220;18
Oroszlán;120;190;13
Leopárd;60;90;15
...
```
## Feladat
```
 Az allatok.csv fájl állatfajok adatait (Állatfaj;Magasság(cm);Súly(kg);Életkor(év))
 tartalmazza, pontosvesszővel elválasztva, utf-8 kódolással.
 Hozzunk létre egy Allatok nevű projektet és oldjuk meg a következő feladatokat!

 0) Olvassuk be a fájl adatait egy megfelelő adatszerkezetbe,
    és jelenítsük meg a beolvasott adatok számát!.....................(2p)
 1) Írjuk ki a legmagasabb állatfajta nevét és magasságát!............(1p)
 2) Határozzuk meg két tizedes jegyre a húsz kilónál nehezebb
    állatfajok átlag életkorát!.......................................(1p)
 3) Keressük meg és írjuk ki az átlagsúlyhoz legközelebbi súlyú
    állatfajta nevét és súlyát!.......................................(2p)
 4) Soroljuk fel azoknak a fajtáknak a nevét,
    melyek nevében egymás után két egyforma betű található!...........(1p)
 5) A magasságuk alapján ötven centinkét kategorizálva a fajtákat,
    határozuk meg mely kategóriákba tartozik állat és hány darab!.....(2p)
    A kategóriákat három számjegyre (vezető nullákkal) írjuk ki!......(1p)
 6) Az előző feladat kategóriái közül válasszunk egyet
    véletlenszerűen, majd soroljuk fel az oda tartozó fajtákat!.......(2p)
    A felsorolásban vessző válassza el a fajtákat (de a végén NE!)....(1p)
 7) Írjuk ki a kicsi.csv fájlba
    az egy méternél kisebb állatok adatait!...........................(2p)

 Minta:
 0) Összesen 26 féle állatfajta adata beolvasva
 1) A legmagasabb állatfajta: Zsiráf, 500cm
 2) A húsz kilónál nehezebb fajták átlagéletkora: 24,71 év
 3) Az átlagsúlyhoz (453kg) legközelebbi fajta: Ló (500kg)
 4) Kettős betű van a fajta nevében:
    * Indiai orrszarvú
    * Gorilla
    * Vaddisznó
 5) Magasság kategórák (50cm):
    * 000-049cm: 4 darab
    * 050-099cm: 8 darab
    * 100-149cm: 5 darab
    * 150-199cm: 7 darab
    * 300-349cm: 1 darab
    * 500-549cm: 1 darab
 6) Ebből egy véletlen kategóriába (0-49cm) eső állatok:
    * Szürke róka, Lemúr, Kakadu, Süni

 kicsi.csv:
 Leopárd;60;90;15
 Farkas;90;40;8
 Szürke róka;40;5;7
 Vörös panda;60;5;10
 Koala;60;14;12
 Lemúr;40;2;17
 Túzok;90;15;9
 Kakadu;30;1;52
 Puma;70;50;12
 Hiúz;80;20;21
 Vaddisznó;80;100;14
 Süni;20;1;7
```
