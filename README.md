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
