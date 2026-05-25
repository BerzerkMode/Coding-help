# Coding-help

# JAVA CLI #

## Osztály létrehozása ##

```
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
```

## Fájl betöltése ##

```
private void betolt(String fajlnev) {
    Scanner be = null;
    try {
        be = new Scanner(new File(fajlnev), "utf-8");
        while (be.hasNextLine()) adatok.add(new Adat(be.nextLine()));
    } catch (FileNotFoundException e) {
        throw new RuntimeException(e);
    } finally {
        if(be != null) be.close();
    }
}

```

## Fájlba kiíratás ##

```
PrintWriter ki = null;
try {
    ki = new PrintWriter(new File("kesok.txt"), "utf-8");
    for(Adat a : adatok) {
        if (a.esemeny == BELEP && a.ido.compareTo("07:50") >  0 && a.ido.compareTo("08:15") <= 0){
            ki.printf("%s %s\r\n", a.ido, a.kod);
        }
    }
} catch (Exception e) {
    throw new RuntimeException(e);
} finally {
    if (ki!= null) ki.close();
}

```
