#include <SoftwareSerial.h> 
#define RX        11    //Serial Receive pin
#define TX        10   //Serial Transmit pin
SoftwareSerial  RS485SerialORBS(RX, TX);
long int RitardoDeye1s;
byte Ritardo;
byte N_buf_8=8;
void setup() {
              Serial.begin(9600);
              RS485SerialORBS.begin(9600); 
              RitardoDeye1s=millis();
                          
} 

void loop()  
{
      if (RitardoDeye1s+5000<millis()){RitardoDeye1s=millis();DEYEModBus_1s();
      }
           

void DEYEModBus_1s() {
Ritardo=40;
//-----------------------------CONSUMO_CASA_185---------------------------------------------
      byte DEYE_request_185[] = {0x0F, 0x03, 0x00,0XB9,0x00,0x01,0x54,0xC1}; 
       RS485SerialORBS.write(DEYE_request_185, sizeof( DEYE_request_185));
       RS485SerialORBS.flush();
      byte DEYE_data_185[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_185,N_buf_8);
      delay(Ritardo);
      for( byte i=0; i<N_buf_8-1; i++ ) {
      Serial.print(DEYE_data_185[i],HEX);Serial.print("_");
      }
      Serial.println("");
    if(DEYE_data_185[0]==15 && DEYE_data_185[1]==3 && DEYE_data_185[2]==2 ) {
      float CONSUMO_CASA_185=(DEYE_data_185[3]*256)+DEYE_data_185[4]; 
      Serial.println("CONSUMO_CASA_185 "+String(CONSUMO_CASA_185));
      }else{Serial.println("CONSUMO_CASA_185  ricezione  dati errore !!"); }
//---------------------------USCITA_BATTERIA_W_190-----------------------------------------------||
      byte DEYE_request_190[] = {0x0F, 0x03, 0x00,0XBE,0x00,0x01,0xE5,0x00}; 
       RS485SerialORBS.write(DEYE_request_190, sizeof( DEYE_request_190));
       RS485SerialORBS.flush();
      byte DEYE_data_190[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_190,N_buf_8); 
      delay(Ritardo);
    for( byte i=0; i<N_buf_8-1; i++ ) {
      Serial.print(DEYE_data_190[i],HEX);
      Serial.print("_");
      }
      Serial.println(" 190 ");
    if(DEYE_data_190[0]==15 && DEYE_data_190[1]==3 && DEYE_data_190[2]==2 ){
      float USCITA_BATTERIA_W_190=(DEYE_data_190[3]*256)+DEYE_data_190[4]; 
      Serial.println("190_USCITA_BATTERIA_W_ string "+String(USCITA_BATTERIA_W_190));
      }else{Serial.println("USCITA_BATTERIA_W_190  ricezione  dati errore !!"); }
//---------------------------SOC 184-----------------------------------------------||
      byte DEYE_request_184[] = {0x0F, 0x03, 0x00,0XB8,0x00,0x01,0x05,0x01}; 
       RS485SerialORBS.write(DEYE_request_184, sizeof( DEYE_request_184));
       RS485SerialORBS.flush();
      byte DEYE_data_184[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_184,N_buf_8); 
  for( byte i=0; i<N_buf_8-1; i++ ) {
      //Serial.print(DEYE_data_184[i],HEX);
      //Serial.print("_");
      }
      //Serial.println(" 184 ");
    if(DEYE_data_184[0]==15 && DEYE_data_184[1]==3 && DEYE_data_184[2]==2 )
      {
      byte SOC=DEYE_data_184[4]; 
      //Serial.println("184 SOC 1 "+String(SOC));
      }else{
            Serial.println("SOC 184  ricezione  dati errore ripete !!");
            }

//--------------------------------------------Grid_external_Total_Power_172------------------------------------ ||
      byte DEYE_request_172[] = {0x0F, 0x03, 0x00,0XAC,0x00,0x01,0x45,0x05}; //SI
       RS485SerialORBS.write(DEYE_request_172, sizeof( DEYE_request_172));
       RS485SerialORBS.flush();
      byte DEYE_data_172[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_172,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_172[0]==15 && DEYE_data_172[1]==3 && DEYE_data_172[2]==2 ){
      float Grid_external_Total_Power_172=(DEYE_data_172[3]*256)+DEYE_data_172[4]; 
      Serial.print("172 Grid_external_Total_Power  ");Serial.println(Grid_external_Total_Power_172);
      }else{Serial.println("Grid_external_Total_Power_172  ricezione  dati errore !!"); }      
//---------------------------PV1_Ingresso 186-----------------------------------------------||
      byte DEYE_request_186[] = {0x0F, 0x03, 0x00,0XBA,0x00,0x01,0xA4,0xC1}; 
       RS485SerialORBS.write(DEYE_request_186, sizeof( DEYE_request_186));
       RS485SerialORBS.flush();
      byte DEYE_data_186[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_186,N_buf_8); 
      delay(Ritardo);
    for( byte i=0; i<N_buf_8-1; i++ ) {
      Serial.print(DEYE_data_186[i]);
      Serial.print("_");}
    if(DEYE_data_186[0]==15 && DEYE_data_186[1]==3 && DEYE_data_186[2]==2 )
      { 
      float PV1_Ingresso_186=(DEYE_data_186[3]*256)+DEYE_data_186[4]; 
      Serial.println("186 Potenza in ingresso PV1 "+String(PV1_Ingresso_186));
      }else{Serial.println("PV1_Ingresso_186  ricezione  dati errore !!"); }
//---------------------------PV2_Ingresso 187-----------------------------------------------||
      byte DEYE_request_187[] = {0x0F, 0x03, 0x00,0XBB,0x00,0x01,0xF5,0x01}; 
       RS485SerialORBS.write(DEYE_request_187, sizeof( DEYE_request_187));
       RS485SerialORBS.flush();
      byte DEYE_data_187[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_187,N_buf_8); 
      delay(Ritardo);
    for( byte i=0; i<N_buf_8-1; i++ ) {
      Serial.print(DEYE_data_187[i]);
      Serial.print("_");}
    if(DEYE_data_187[0]==15 && DEYE_data_187[1]==3 && DEYE_data_187[2]==2 )
      {
      float PV2_Ingresso_187=(DEYE_data_187[3]*256)+DEYE_data_187[4]; 
      }else{Serial.println("PV2_Ingresso_187  ricezione  dati errore !!"); }    


//--------------------------RUN_STATE_59----------------------------------||
      byte DEYE_request_59[] = {0x0F, 0x03, 0x00,0X3B,0x00,0x01,0xF4,0xE9}; 
       RS485SerialORBS.write(DEYE_request_59, sizeof( DEYE_request_59));
       RS485SerialORBS.flush();
      byte DEYE_data_59[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_59,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_59[0]==15 && DEYE_data_59[1]==3 && DEYE_data_59[2]==2 )
      { 
      float RUN_STATE_59=DEYE_data_59[4]; 
      Serial.println("59 RUN_STATE "+String(RUN_STATE_59));
      }else{Serial.println("RUN_STATE_59   ricezione  dati errore !!");}

//--------------------------------TENSIONE_BATT 183------------------------------------------------ ||
      byte DEYE_request_183[] = {0x0F, 0x03, 0x00,0XB7,0x00,0x01,0x35,0x02}; 
       RS485SerialORBS.write(DEYE_request_183, sizeof( DEYE_request_183));
       RS485SerialORBS.flush();
      byte DEYE_data_183[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_183,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_183[0]==15 && DEYE_data_183[1]==3 && DEYE_data_183[2]==2 )
      { 
      float TENSIONE_BATT=(DEYE_data_183[3]*256)+DEYE_data_183[4]; 
      TENSIONE_BATT=TENSIONE_BATT/100;
      Serial.print("183 TENSIONE_BATT  ");Serial.println(TENSIONE_BATT);
      }else{
      Serial.println("TENSIONE_BATT 183  ricezione  dati errore !!");
      }

//---------------------------------Day_Batt_Charge_PowerWh_70----------------- 
      byte DEYE_request_70[] = {0x0F, 0x03, 0x00,0X46,0x00,0x01,0x64,0xF1}; 
       RS485SerialORBS.write(DEYE_request_70, sizeof( DEYE_request_70));
       RS485SerialORBS.flush();
      byte DEYE_data_70[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_70,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_70[0]==15 && DEYE_data_70[1]==3 && DEYE_data_70[2]==2 ){
      float Day_Batt_Charge_PowerWh_70=(DEYE_data_70[3]*256)+DEYE_data_70[4]; 
      Serial.println("70 Day_Batt_Charge _Power_KWh "+String(Day_Batt_Charge_PowerWh_70/10));   //  DIVISO 10
      }else{Serial.println("Day_Batt_Charge_PowerWh_70  ricezione  dati errore !!"); }
//---------------------------------Day_Batt_Discharge_PowerWh_71 -----------------  
      byte DEYE_request_71[] = {0x0F, 0x03, 0x00,0X47,0x00,0x01,0x35,0x31}; 
       RS485SerialORBS.write(DEYE_request_71, sizeof( DEYE_request_71));
       RS485SerialORBS.flush();
      byte DEYE_data_71[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_71,N_buf_8);
      delay(Ritardo); 
    if(DEYE_data_71[0]==15 && DEYE_data_71[1]==3 && DEYE_data_71[2]==2 ){
      float Day_Batt_Discharge_PowerWh_71=(DEYE_data_71[3]*256)+DEYE_data_71[4];
      //Serial.println("71 Day_Batt_Discharge_Power_KWh "+String(Day_Batt_Discharge_PowerWh_71/10));   //  DIVISO 10
      //Serial.println("71 Day_Batt_Discharge_Power_KWh "+String(DEYE_data_71[3])); 
      //Serial.println("71 Day_Batt_Discharge_Power_KWh "+String(DEYE_data_71[4])); 
      }//else{Serial.println("Day_Batt_Discharge_PowerWh_71  ricezione  dati errore !!"); }
//---------------------------------Day_PVPower_Wh_108  ------------------------------
      byte DEYE_request_108[] = {0x0F, 0x03, 0x00,0X6C,0x00,0x01,0x45,0x39}; 
       RS485SerialORBS.write(DEYE_request_108, sizeof( DEYE_request_108));
       RS485SerialORBS.flush();
      byte DEYE_data_108[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_108,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_108[0]==15 && DEYE_data_108[1]==3 && DEYE_data_108[2]==2 ){
      float Day_PVPower_KWh_108=(DEYE_data_108[3]*256)+DEYE_data_108[4]; 
      //Serial.println("108 Day_PV PRODUZIONE_KWh  "+String(Day_PVPower_KWh_108/10));
      }//else{Serial.println("Day_PVPower_KWh_108  ricezione  dati errore !!");}

//--------------------------DayActive_PowerWh_60----------------------------------
      byte DEYE_request_60[] = {0x0F, 0x03, 0x00,0X3C,0x00,0x01,0x45,0x28}; 
       RS485SerialORBS.write(DEYE_request_60, sizeof( DEYE_request_60));
       RS485SerialORBS.flush();
      byte DEYE_data_60[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_60,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_60[0]==15 && DEYE_data_60[1]==3 && DEYE_data_60[2]==2 )
      { 
      float  DayActive_PowerWh_60=(DEYE_data_60[3]*256)+DEYE_data_60[4];
      //Serial.println("60 Day_CONSUMO_CASA_KWh "+String(DayActive_PowerWh_60/10));
      }//else{Serial.println("DayActive_PowerWh_60 ricezione  dati errore !!"); }

      
//---------------------------------Day_GridBuy_Power_Wh_76  ----------------- 
      byte DEYE_request_76[] = {0x0F, 0x03, 0x00,0X4C,0x00,0x01,0x44,0xF3}; 
       RS485SerialORBS.write(DEYE_request_76, sizeof( DEYE_request_76));
       RS485SerialORBS.flush();
      byte DEYE_data_76[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_76,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_76[0]==15 && DEYE_data_76[1]==3 && DEYE_data_76[2]==2 ){
      float Day_GridBuy_Power_Wh_76=(DEYE_data_76[3]*256)+DEYE_data_76[4]; 
      //Serial.println("76 Day_GridBuy_Power_KWh "+String(Day_GridBuy_Power_Wh_76));
      } //else{Serial.println("TENSIONE_BATT 183  ricezione  dati errore !!");}
//---------------------------------Day_GridSell_Power_Wh_77-----------------  
      byte DEYE_request_77[] = {0x0F, 0x03, 0x00,0X4D,0x00,0x01,0x15,0x33}; 
       RS485SerialORBS.write(DEYE_request_77, sizeof( DEYE_request_77));
       RS485SerialORBS.flush();
      byte DEYE_data_77[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_77,N_buf_8);
      delay(Ritardo);
    if(DEYE_data_77[0]==15 && DEYE_data_77[1]==3 && DEYE_data_77[2]==2 ){
      float Day_GridSell_Power_Wh_77=(DEYE_data_77[3]*256)+DEYE_data_77[4]; 
      Serial.println("77 Day_GridSell_Power_KWh "+String(Day_GridSell_Power_Wh_77));    
      }else{Serial.println("Day_GridBuy_Power_Wh_76  ricezione  dati errore !!");}
    

//--------------------------------------------Grid_external_Total_Power_172------------------------------------ ||
      byte DEYE_request_172[] = {0x0F, 0x03, 0x00,0XAC,0x00,0x01,0x45,0x05}; //SI
       RS485SerialORBS.write(DEYE_request_172, sizeof( DEYE_request_172));
       RS485SerialORBS.flush();
      byte DEYE_data_172[N_buf_8]={};
       RS485SerialORBS.readBytes(DEYE_data_172,N_buf_8); 
      delay(Ritardo);
    if(DEYE_data_172[0]==15 && DEYE_data_172[1]==3 && DEYE_data_172[2]==2 ){
      float Grid_external_Total_Power_172=(DEYE_data_172[3]*256)+DEYE_data_172[4]; 
      Serial.print("172 Grid_external_Total_Power  ");Serial.println(Grid_external_Total_Power_172);
      }else{Serial.println("Grid_external_Total_Power_172  ricezione  dati errore !!");}
}


      
