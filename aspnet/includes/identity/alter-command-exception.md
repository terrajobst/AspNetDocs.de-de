> Einige Befehle werden nicht unterstützt, wenn die APP SQLite als Identitätsdaten Speicher verwendet. Aufgrund von Einschränkungen in der Datenbank-Engine lösen `Alter` Befehle folgende Ausnahme aus:
>
> "System. NotSupportedException: dieser Migrations Vorgang wird von SQLite nicht unterstützt." 
>
> Führen Sie als Problem Umgehung Code First Migrationen in der Datenbank aus, um die Tabellen zu ändern.
