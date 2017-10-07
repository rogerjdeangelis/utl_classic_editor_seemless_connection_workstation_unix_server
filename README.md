# utl_classic_editor_seemless_connection_workstation_unix_server
Persistent SSH SAS TCIP workstation to unix server. One classic SAS interactive development enviromnet. SAS display maamner. SAS connect. Command macros. mouse action and function keys that work locally and on the unix server.

    ```  How to switch environments automatically using SAS EG  ```
    ```    ```
    ```    ```
    ```    Single stream of code  ```
    ```    ```
    ```    * WORKSTATION CODE;  ```
    ```       data local_class;  ```
    ```         set sashelp.class;  ```
    ```       run;quit;  ```
    ```    ```
    ```    * SERVER CODE;  ```
    ```     rsubmit;  ```
    ```       data remote_class;  ```
    ```         set sashelp.class;  ```
    ```       run;quit;  ```
    ```     endrsubmit  ```
    ```    ```
    ```    ```
    ```  Seemless Interative Unix Server/Local Worstation development enviromnet  ```
    ```    ```
    ```  I have two versions of my command macros, that can executed on the command line, on function keys or  ```
    ```  of mouse mappings.  ```
    ```    ```
    ```  One set of command macros interact with a 'persistent' SAS connection(SAS connect) to a unix server and a  ```
    ```  second set work on my power workstation.  ```
    ```    ```
    ```  For instance  ```
    ```    ```
    ```    SERVER If I highlight a unix dataset 'remote.class' in any display manager window and type  ```
    ```    ```
    ```         xfrqh sex  ```
    ```    ```
    ```          A proc frequency will be run on the server for the highlighted dataset  ```
    ```          in the server work directory and the results will appear in  my local SAS output window  ```
    ```    ```
    ```    WORSTATION  ```
    ```    ```
    ```          If I highlight a workstation dataset in any display manager window and type  ```
    ```    ```
    ```          frqh  ```
    ```    ```
    ```          A proc frequency will be run on the workstation 'local_class' dataset for the highlighted dataset  ```
    ```          and the results will appear in  my local SAS output window  ```
    ```    ```
    ```    ```
    ```     I use 'unx' for the unix work directory so you can do things like  ```
    ```    ```
    ```        data local_class;  ```
    ```           set unx.class;  ```
    ```        run;quit;  ```
    ```    ```
    ```    ```
    ```      I got a call once from the IT police saying a was used more cpu than any of the other alost 100  ```
    ```      SAS programmers. I said great, my command macros must be working hard for me.  ```
    ```    ```
    ```    ```
    ```  SERVER COMMAND MACRO  ```
    ```  =====================  ```
    ```    ```
    ```    ```
    ```  %macro xfrqh /cmd parmbuff;  ```
    ```  store;  ```
    ```  /*-----------------------------------------*\  ```
    ```  |  frq sex*age                              |  ```
    ```  \*-----------------------------------------*/  ```
    ```     %let argx=%scan(&syspbuff,1,%str( ));  ```
    ```     %syslput argx=&argx;  ```
    ```     note;notesubmit '%xfrqa;';  ```
    ```     run;  ```
    ```  %mend xfrqh;  ```
    ```  %macro xfrqa;  ```
    ```     filename clp clipbrd ;  ```
    ```     data _null_;  ```
    ```       infile clp;  ```
    ```       input;  ```
    ```       put _infile_;  ```
    ```       call symputx('argy',_infile_);  ```
    ```     run;  ```
    ```     %syslput argy=&argy;  ```
    ```     dm "out;clear;";  ```
    ```     rsubmit;   ** run on server; ```
    ```     options nocenter;  ```
    ```     footnote;  ```
    ```     title1 "frequency of &argx datasets &argd";  ```
    ```     proc freq data=&argd levels;  ```
    ```     tables &argx./list missing;  ```
    ```     run;  ```
    ```     title;  ```
    ```     endrsubmit;  ```
    ```     dm "out;top;";  ```
    ```  %mend xfrqa;  ```
    ```    ```
    ```    ```
    ```    ```
    ```  WORKSTATION COMMAND MACRO  ```
    ```  ==========================  ```
    ```    ```
    ```  %macro frqh /cmd parmbuff;  ```
    ```     %let argx=%scan(&syspbuff,1,%str( ));  ```
    ```     store;note;notesubmit '%frqha;';  ```
    ```     run;  ```
    ```  %mend frqh;  ```
    ```    ```
    ```  %macro frqha;  ```
    ```     filename clp clipbrd ;  ```
    ```     data _null_;  ```
    ```       infile clp;  ```
    ```       input;  ```
    ```       put _infile_;  ```
    ```       call symputx('argd',_infile_);  ```
    ```     run;  ```
    ```     dm "out;clear;";  ```
    ```     options nocenter;  ```
    ```     footnote;  ```
    ```     title1 "frequency of &argx datasets &argd";  ```
    ```     proc freq data=&argd levels;  ```
    ```     tables &argx./list missing;  ```
    ```     run;  ```
    ```     title;  ```
    ```     dm "out;top;";  ```
    ```  %mend frqha;  ```
    ```    ```
    ```    ```
