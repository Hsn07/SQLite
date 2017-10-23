package com.example.hsn07.yoklamalistesi;

import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.nfc.Tag;
import android.util.Log;
import android.widget.Toast;

import java.util.ArrayList;

/**
 * Created by Hsn07 on 25.9.2017.
 */

public class Database extends SQLiteOpenHelper {
    private static final String DATABASE_NAME="Talebe";
    private static final int DATABASE_VERSION=1;

    public Database(Context context){
        super(context,DATABASE_NAME,null,DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String CREATE_TALEBELISTESI="CREATE TABLE Talebelistesi("+"id VARCHAR , " +
                "ad VARCHAR , " +
                "soyad VARCHAR , " +
                "telefon VARCHAR , " +
                "bolum VARCHAR , " +
                "sinif VARCHAR , " +
                "adet VARCHAR)";
        db.execSQL(CREATE_TALEBELISTESI);
        String CREATE_GELMEYENTALEBELISTESI="CREATE TABLE GelmeyenTalebelistesi("+"id VARCHAR , " +
                "ad VARCHAR , " +
                "soyad VARCHAR , " +
                "saat VARCHAR , " +
                "tarih VARCHAR , " +
                "adet VARCHAR)";
        db.execSQL(CREATE_GELMEYENTALEBELISTESI);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int i, int i1) {
        db.execSQL("DROP TABLE IF EXITS Talebelistesi");
        onCreate(db);
    }
    public void yeni_talebe(Talebe talebe){
        SQLiteDatabase db=this.getWritableDatabase();
        String INSERT_TALEBE_SQL=("INSERT INTO Talebelistesi(id,ad,soyad,telefon,bolum,sinif,adet)"+
                "VALUES ('"+talebe.getId()+"','"+talebe.getAd()+"','"+talebe.getSoyad()+"','"+talebe.getTelefon()+"','"+talebe.getBolum()+"','"+talebe.getSinif()+"','"+0+"')");
        db.execSQL(INSERT_TALEBE_SQL);

        db.close();
    }
    public Talebe getir_kisi(String id){
        SQLiteDatabase db=this.getWritableDatabase();
        String SELECT_KISI_ID="SELECT id,ad,soyad,telefon,bolum,sinif,adet FROM Talebelistesi WHERE id= "+id;
        Cursor cursor=db.rawQuery(SELECT_KISI_ID,null);

        if (cursor!=null){
            cursor.moveToFirst();
        }
        Talebe talebe=new Talebe();
        talebe.setId(cursor.getString(0));
        talebe.setAd(cursor.getString(1));
        talebe.setSoyad(cursor.getString(2));
        talebe.setTelefon(cursor.getString(3));
        talebe.setBolum(cursor.getString(4));
        talebe.setSinif(cursor.getString(5));
        talebe.setAdet(cursor.getString(6));

        return talebe;
    }
    public ArrayList<Talebe> talebelistesi(){
        SQLiteDatabase db=this.getWritableDatabase();

        String SELECT_TALEBELER="SELECT * FROM Talebelistesi";
        Cursor cursor=db.rawQuery(SELECT_TALEBELER,null);

        ArrayList<Talebe> talebelist=new ArrayList<Talebe>();

        if(cursor!=null){
            cursor.moveToFirst();
            do {
                Talebe tlb=new Talebe();
                tlb.setId(cursor.getString(0));
                tlb.setAd(cursor.getString(1));
                tlb.setSoyad(cursor.getString(2));
                tlb.setTelefon(cursor.getString(3));
                tlb.setBolum(cursor.getString(4));
                tlb.setSinif(cursor.getString(5));
                tlb.setAdet(cursor.getString(6));
                talebelist.add(tlb);
            }while (cursor.moveToNext());
        }
        return talebelist;
    }
    public void talebegüncelle(GTalebe gtalebe){
        SQLiteDatabase db=this.getWritableDatabase();

        String UPPDATE_TALEBE="UPDATE Talebelistesi SET adet = " + gtalebe.getAdet() + " WHERE id = " +gtalebe.getId();

            db.execSQL(UPPDATE_TALEBE);

        db.close();

    }
    public void talebesil(String id){
        SQLiteDatabase db=this.getWritableDatabase();

        String DELETE_TALEBE="DELETE FROM Talebelistesi WHERE id="+id;

        db.execSQL(DELETE_TALEBE);

        db.close();
    }
    public void yeni_gtalebe(GTalebe gtalebe){
        SQLiteDatabase db=this.getWritableDatabase();
        String INSERT_GTALEBE_SQL=("INSERT INTO GelmeyenTalebelistesi(id,ad,soyad,saat,tarih,adet)"+
                "VALUES ('"+gtalebe.getId()+"','"+gtalebe.getAd()+"','"+gtalebe.getSoyad()+"','"+gtalebe.getSaat()+"','"+gtalebe.getTarih()+"','"+gtalebe.getAdet()+"')");
        db.execSQL(INSERT_GTALEBE_SQL);

        db.close();
    }
    public ArrayList<GTalebe> gtalebelistesi(){
        SQLiteDatabase db=this.getWritableDatabase();

        String SELECT_TALEBELER="SELECT * FROM GelmeyenTalebelistesi";
        Cursor cursor=db.rawQuery(SELECT_TALEBELER,null);

        ArrayList<GTalebe> gtalebelist=new ArrayList<GTalebe>();
        if(cursor!=null){
            cursor.moveToLast();
            do {
                GTalebe gtlb=new GTalebe();
                gtlb.setId(cursor.getString(0));
                gtlb.setAd(cursor.getString(1));
                gtlb.setSoyad(cursor.getString(2));
                gtlb.setSaat(cursor.getString(3));
                gtlb.setTarih(cursor.getString(4));
                gtlb.setAdet(cursor.getString(5));
                gtalebelist.add(gtlb);
            }while (cursor.moveToPrevious());
        }
        return gtalebelist;
    }
}
