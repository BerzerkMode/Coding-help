# Coding-help

# JAVA CLI #

### Osztály létrehozása ###

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

### Fájl betöltése ###

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

### Fájlba kiíratás ###

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

# JAVA GUI #

### Inicializálás ###

 ```
  private FileChooser fc = new FileChooser();

    public void initialize(){
        fc.setInitialDirectory(new File("./"));
        fc.getExtensionFilters().add(new FileChooser.ExtensionFilter("CSV fájlok", "*.csv"));
        lslista.setItems(fovarosok);
    }

```

### Fájl betöltés ###

```
private void betolt(File fajl){
        Scanner be = null;
        try {
            be = new Scanner(fajl, "utf-8");
            be.nextLine();
            while(be.hasNextLine()) fovarosok.add(new Fovaros(be.nextLine()));
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            if( be != null) be.close();
        }
    }

```

### Névjegy ###

```
@FXML private void onNevjegyClick(){
        Alert info = new Alert(Alert.AlertType.INFORMATION);
        info.setTitle("Rajzok / Névjegy");
        info.setContentText("Rajzok v1.0.0\n(C) 2024");
        info.setHeaderText(null);
        info.showAndWait();
    }

```

### Példa ###

```
package com.example.listak;

import javafx.fxml.FXML;
import javafx.scene.control.Label;
import javafx.scene.control.ListView;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.io.UnsupportedEncodingException;
import java.util.Scanner;

public class HelloController {

   @FXML private Label lbInfo;
   @FXML private Label lbKitty;
   @FXML private Label lbGomba;
   @FXML private Label lbMadar;
   @FXML private ListView<String> lsBal;
   @FXML private ListView<String> lsJobb;

   private int dbKitty = 0;
   private int dbGomba = 0;
   private int dbMadar = 0;

   public void initialize() {
      File fbe = new File("listak.txt");
      if (fbe.exists()) betolt(fbe);
   }

   @FXML private void onKittyClick(){ lsBal.getItems().add("Kitty"); dbKitty++; setInfo(); }
   @FXML private void onGombaClick(){ lsBal.getItems().add("Gomba"); dbGomba++; setInfo(); }
   @FXML private void onMadarClick(){ lsBal.getItems().add("Madár"); dbMadar++; setInfo(); }
   @FXML private void onKukaBalClick(){
      lsBal.getItems().clear();
      dbKitty = 0; dbGomba = 0; dbMadar = 0; setInfo();
   }

   private void setInfo() {
      lbKitty.setText(dbKitty+"");
      lbGomba.setText(dbGomba+"");
      lbMadar.setText(dbMadar+"");
      lbInfo.setText(lsBal.getItems().size() + " / " + lsJobb.getItems().size() + " elem van a listákban");
   }

   @FXML private void  onAddClick() {
      int i = lsBal.getSelectionModel().getSelectedIndex();
      if (i != -1){
         String nev = lsBal.getItems().get(i);
         lsJobb.getItems().add(lsBal.getItems().get(i));
         lsBal.getItems().remove(i);
         if(nev.equals("Kitty")) dbKitty--; else if (nev.equals("Gomba")) dbGomba--; else dbMadar--   ;
         setInfo();
      }
   }

   @FXML private void onDelClick() {
      int i = lsJobb.getSelectionModel().getSelectedIndex();
      if( i!= -1){
         lsJobb.getItems().remove(i); setInfo();
      }
   }

   @FXML private void onKukaJobbClick(){
      lsJobb.getItems().clear();
      setInfo();
   }

   @FXML private void onSaveClick() {
      mentes("listak.txt");
   }

   private void mentes(String fajlnev) {
      PrintWriter ki = null;
       try {
           ki = new PrintWriter(new File(fajlnev), "utf-8");
           for (int i=0; i<lsBal.getItems().size(); i++) ki.printf("%s\r\n", lsBal.getItems().get(i));
           ki.printf("\r\n");
          for (int i=0; i<lsJobb.getItems().size(); i++) ki.printf("%s\r\n", lsJobb.getItems().get(i));
       } catch (Exception e) {
           throw new RuntimeException(e);
       } finally {
          if (ki !=null ) ki.close();
       }
   }
   private void betolt(File fajl) {
      Scanner be = null;
       try {
           be = new Scanner(fajl, "utf-8");
           String sor = "";
           do {
              sor = be.nextLine();
              if (!sor.equals("")) lsBal.getItems().add(sor);
           } while (!sor.equals(""));
           while (be.hasNextLine()) lsJobb.getItems().add(be.nextLine());
       } catch (FileNotFoundException e) {
           throw new RuntimeException(e);
       } finally {
          if (be != null) be.close();
       }
   }
}

```

# Backend # 

### Importálás ###

```
pnpm init //cmd
pnpm i express cors mysql2

import express from "express";
import cors from "cors";
import mysql from "mysql2/promise";

const app = express();
app.use(express.json());
app.use(cors());
```

### MySQL Connection ### 

```
const con = await mysql.createConnection({
  host: "localhost",
  port: 3306,
  database: "adatbazis", 
  user: "root",
  password: ""
});

```

### Végpontok ###

```
app.get("/", (req, res) => res.send("<h1>Lepkék v1.0.0</h1>"));
app.get("/lepkek", getLepkek);
app.post("/lepke", postLepke);
app.patch("/lepke/:id/:sebesseg", patchLepke);
app.delete("/lepke/:id", deleteLepke);

app.listen(88, err => console.log(err ? err : "Server on #88"));

```

### Függvények ###

```
async function getZenek(req, res) {
    try {
        let sql = "select * from zenek order by cim";
        const [ json ] = await con.query(sql);
        res.send(json);
    } catch(err) { res.status(500).send({ error:"Szerver hiba!" }); }
}

async function postZene(req, res) {
    let { eloado, cim, hossz } = req.body;
    if (eloado && cim && hossz) {
        try {
            let sql = "insert into zenek set eloado=?, cim=?, hossz=?";
            const [ json ] = await con.query(sql, [eloado, cim, hossz]);
            res.status(201).send({ status:"OK" });
        } catch(err) { res.status(500).send({ error:"Szerver hiba!" }); }
    } else res.status(400).send({ error:"Hibás paraméter!"});
}

async function putZene(req, res) {
    let { id } = req.params;
    let { eloado, cim, hossz } = req.body;
    if (eloado && cim && hossz) {
        try {
            let sql = "update zenek set eloado=?, cim=?, hossz=? where id=?";
            const [ json ] = await con.query(sql, [eloado, cim, hossz, id]);
            if (json.affectedRows != 0) res.send({ status:"OK" });
            else res.status(404).send({ error:"Nincs ilyen!" });
        } catch(err) { res.status(500).send({ error:"Szerver hiba!" }); }
    } else res.status(400).send({ error:"Hibás paraméter!"});
}

async function deleteZene(req, res) {
    let { id } = req.params;
    try {
        let sql = "delete from zenek where id=?";
        const [ json ] = await con.query(sql, [id]);
        if (json.affectedRows != 0) res.send({ status:"OK" });
        else res.status(404).send({ error:"Nincs ilyen!" });
    } catch(err) { res.status(500).send({ error:"Szerver hiba!" }); }
}

```

# Frontend #

### Importálás ###

```
pnpm i axios, react-router-dom //cmd
import axios from "axios";

//Példa

import { useState } from 'react'
import './App.css'
import Bekero from './Bekero'
import Szuro from './Szuro'
import Gyujto from './Gyujto';
```

[https://github.com/barsonyj/help](https://github.com/barsonyj/help)


