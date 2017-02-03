unit OtForm;

interface

uses
  Windows, Messages, SysUtils, Classes, Graphics, Controls, Forms, Dialogs,
  ImgList, ExtCtrls, Abcwav, StdCtrls, Buttons, RzButton, registry, Mask,
  RzEdit, RzSpnEdt;

type
  TForm1 = class(TForm)
    Image1: TImage;
    il: TImageList;
    w1: TabcWave;
    w2: TabcWave;
    Image2: TImage;
    Image3: TImage;
    Label1: TLabel;
    Label2: TLabel;
    RzToolbarButton1: TRzToolbarButton;
    RzToolbarButton2: TRzToolbarButton;
    GroupBox1: TGroupBox;
    Label5: TLabel;
    Label6: TLabel;
    fs: TRzSpinEdit;
    Label3: TLabel;
    procedure Pnt;
    procedure PCA;
    function Test(Xa,Ya: integer; Ex: boolean): boolean;
    function TestPC(Xa,Ya: integer): boolean;
    procedure FormCreate(Sender: TObject);
    procedure Image1MouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    procedure RzToolbarButton1Click(Sender: TObject);
    procedure RzToolbarButton2Click(Sender: TObject);
    procedure FormClose(Sender: TObject; var Action: TCloseAction);
    procedure fsChange(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  Form1: TForm1;
  mx: array[0..15] of array[0..15] of byte;
  ab: boolean;
  hi1,hi2: byte;

implementation

{$R *.DFM}

procedure TForm1.Pnt;
var x,y,xc,yc: integer;
begin
  xc:=0;
  yc:=0;
  for x:=0 to fs.IntValue-1 do
  for y:=0 to fs.IntValue-1 do begin
    if mx[x,y]=1 then il.draw(Image1.Canvas,3+30*x,3+30*y,1);
    if mx[x,y]=2 then il.draw(Image1.Canvas,3+30*x,3+30*y,0);
    if mx[x,y]=1 then Inc(xc);
    if mx[x,y]=2 then Inc(yc);
  end;
  Label1.Caption:=IntToStr(yc);
  Label2.Caption:=IntToStr(xc);
  Image1.Repaint;
end;

procedure HiLoad;
var r: TRegistry;
begin
  hi1:=2;
  hi2:=2;
  r:=TRegistry.Create;
  r.RootKey:=HKEY_LOCAL_MACHINE;
  r.OpenKey('\SOFTWARE\DIS Soft\Games\Othello',true);
  if r.ValueExists('MyScore') then begin
    Form1.Label5.Caption:=r.ReadString('Name');
    hi1:=r.ReadInteger('MyScore');
    hi2:=r.ReadInteger('PCScore');
    Form1.Label6.Caption:=IntToStr(hi1)+' : '+IntToStr(hi2);
  end;
  r.CloseKey;
end;

procedure HiSave;
var r: TRegistry;
begin
  r:=TRegistry.Create;
  r.RootKey:=HKEY_LOCAL_MACHINE;
  r.OpenKey('\SOFTWARE\DIS Soft\Games\Othello',true);
  r.WriteString('Name',Form1.Label5.Caption);
  r.WriteInteger('MyScore',hi1);
  r.WriteInteger('PCScore',hi2);
  r.CloseKey;
end;

procedure TForm1.FormCreate(Sender: TObject);
begin
  HiLoad;
  Image1.Picture.LoadFromFile(ExtractFileDir(ParamStr(0))+'\BKG.BMP');
  mx[fs.IntValue div 2 - 1,fs.IntValue div 2 - 1]:=2;
  mx[fs.IntValue div 2,fs.IntValue div 2]:=2;
  mx[fs.IntValue div 2 - 1,fs.IntValue div 2]:=1;
  mx[fs.IntValue div 2,fs.IntValue div 2 - 1]:=1;
  il.draw(Image2.Canvas,0,0,0);
  il.draw(Image3.Canvas,0,0,1);
  Pnt;
end;

function TForm1.Test(Xa,Ya: integer; Ex: boolean): boolean;
var x,y: integer;
    e: integer;
    arr: array of record x,y: byte; end;
begin
  e:=-1;
  // Vertical Down
  if ya<(fs.IntValue-2) then begin
   y:=ya+1;
   if mx[xa,y]=1 then
   repeat
     if mx[xa,y]=2 then begin
       e:=y;
       y:=fs.IntValue-1;
     end;
     if mx[xa,y]=0 then y:=fs.IntValue-1;
     Inc(y);
   until y>(fs.IntValue-1);
   if e>=0 then
     for y:=ya+1 to e-1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=xa;
      arr[length(arr)-1].y:=y;
     end;
  end;
  e:=-1;
  //Vertical Up
  if ya>1 then begin
   y:=ya-1;
   if mx[xa,y]=1 then
   repeat
     if mx[xa,y]=2 then begin
       e:=y;
       y:=0;
     end;
     if mx[xa,y]=0 then y:=0;
     Dec(y);
   until y<0;
   if e>=0 then
     for y:=ya-1 downto e+1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=xa;
      arr[length(arr)-1].y:=y;
     end;
  end;
  e:=-1;
  //Horizontal Right
  if xa<(fs.IntValue-2) then begin
   x:=xa+1;
   if mx[x,ya]=1 then
   repeat
     if mx[x,ya]=2 then begin
       e:=x;
       x:=9;
     end;
     if mx[x,ya]=0 then x:=fs.IntValue-1;
     Inc(x);
   until x>(fs.IntValue-1);
   if e>=0 then
     for x:=xa+1 to e-1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=ya;
     end;
  end;
  e:=-1;
  // Hirizontal Left
  if xa>1 then begin
   x:=xa-1;
   if mx[x,ya]=1 then
   repeat
     if mx[x,ya]=2 then begin
       e:=x;
       x:=-1;
     end;
     if mx[x,ya]=0 then x:=0;
     Dec(x);
   until x<0;
   if e>=0 then
     for x:=xa-1 downto e+1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=ya;
     end;
  end;
  e:=-1;
  // Main Diagonal End
  if (xa<(fs.IntValue-2)) and (ya<(fs.IntValue-2)) then begin
    /////////////
   x:=xa+1;
   y:=ya+1;
   if mx[x,y]=1 then
   repeat
     if mx[x,y]=2 then begin
       e:=x;
       x:=9;
     end;
     if mx[x,y]=0 then x:=fs.IntValue-1;
     Inc(x);
     Inc(y);
   until (x>(fs.IntValue-1)) or (y>(fs.IntValue-1));
   if e>=0 then
     for x:=xa+1 to e-1 do begin
      y:=ya+(x-xa);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  e:=-1;
  // Main Diagonal Begin
  if (xa>1) and (ya>1) then begin
    /////////////
   x:=xa-1;
   y:=ya-1;
   if mx[x,y]=1 then
   repeat
     if mx[x,y]=2 then begin
       e:=x;
       x:=-1;
     end;
     if mx[x,y]=0 then x:=-1;
     Dec(x);
     Dec(y);
   until (x<0) or (y<0);
   if e>=0 then
     for x:=xa-1 downto e+1 do begin
      y:=ya-(xa-x);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  e:=-1;
  //Sec Diag End
  if (xa>1) and (ya<(fs.IntValue-2)) then begin
    /////////////
   x:=xa-1;
   y:=ya+1;
   if mx[x,y]=1 then
   repeat
     if mx[x,y]=2 then begin
       e:=x;
       x:=0;
     end;
     if mx[x,y]=0 then x:=0;
     Dec(x);
     Inc(y);
   until (x<0) or (y>(fs.IntValue-1));
   if e>=0 then
     for x:=xa-1 downto e+1 do begin
      y:=ya+(xa-x);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  e:=-1;
  //Sec Diag Begin
  if (ya>1) and (xa<(fs.IntValue-2)) then begin
    /////////////
   x:=xa+1;
   y:=ya-1;
   if mx[x,y]=1 then
   repeat
     if mx[x,y]=2 then begin
       e:=x;
       x:=9;
     end;
     if mx[x,y]=0 then x:=fs.IntValue-1;
     Inc(x);
     Dec(y);
   until (y<0) or (x>(fs.IntValue-1));
   if e>=0 then
     for x:=xa+1 to e-1 do begin
      y:=ya+(xa-x);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  //Final
  if length(arr)>0 then begin
   if not ex then for e:=0 to length(arr)-1 do mx[arr[e].x,arr[e].y]:=2;
   result:=true;
  end else begin
    mx[xa,ya]:=0;
    result:=false;
  end;
end;

function TForm1.TestPC(Xa,Ya: integer): boolean;
var x,y: integer;
    e: integer;
    arr: array of record x,y: byte; end;
begin
  e:=-1;
  // Vertical Down
  if ya<(fs.IntValue-2) then begin
   y:=ya+1;
   if mx[xa,y]=2 then
   repeat
     if mx[xa,y]=1 then begin
       e:=y;
       y:=fs.IntValue-1;
     end;
     Inc(y);
   until y>(fs.IntValue-1);
   if e>=0 then
     for y:=ya+1 to e-1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=xa;
      arr[length(arr)-1].y:=y;
     end;
  end;
  e:=-1;
  //Vertical Up
  if ya>1 then begin
   y:=ya-1;
   if mx[xa,y]=2 then
   repeat
     if mx[xa,y]=1 then begin
       e:=y;
       y:=-1;
     end;
     Dec(y);
   until y<0;
   if e>=0 then
     for y:=ya-1 downto e+1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=xa;
      arr[length(arr)-1].y:=y;
     end;
  end;
  e:=-1;
  //Horizontal Right
  if xa<(fs.IntValue-2) then begin
   x:=xa+1;
   if mx[x,ya]=2 then
   repeat
     if mx[x,ya]=1 then begin
       e:=x;
       x:=fs.IntValue-1;
     end;
     Inc(x);
   until x>(fs.IntValue-1);
   if e>=0 then
     for x:=xa+1 to e-1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=ya;
     end;
  end;
  e:=-1;
  // Hirizontal Left
  if xa>1 then begin
   x:=xa-1;
   if mx[x,ya]=2 then
   repeat
     if mx[x,ya]=1 then begin
       e:=x;
       x:=-1;
     end;
     Dec(x);
   until x<0;
   if e>=0 then
     for x:=xa-1 downto e+1 do begin
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=ya;
     end;
  end;
  e:=-1;
  // Main Diagonal End
  if (xa<(fs.IntValue-2)) and (ya<(fs.IntValue-2)) then begin
    /////////////
   x:=xa+1;
   y:=ya+1;
   if mx[x,y]=2 then
   repeat
     if mx[x,y]=1 then begin
       e:=x;
       x:=fs.IntValue-1;
     end;
     Inc(x);
     Inc(y);
   until (x>(fs.IntValue-1)) or (y>(fs.IntValue-1));
   if e>=0 then
     for x:=xa+1 to e-1 do begin
      y:=ya+(x-xa);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  e:=-1;
  // Main Diagonal Begin
  if (xa>1) and (ya>1) then begin
    /////////////
   x:=xa-1;
   y:=ya-1;
   if mx[x,y]=2 then
   repeat
     if mx[x,y]=1 then begin
       e:=x;
       x:=-1;
     end;
     Dec(x);
     Dec(y);
   until (x<0) or (y<0);
   if e>=0 then
     for x:=xa-1 downto e+1 do begin
      y:=ya-(xa-x);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  e:=-1;
  //Sec Diag End
  if (xa>1) and (ya<(fs.IntValue-2)) then begin
    /////////////
   x:=xa-1;
   y:=ya+1;
   if mx[x,y]=2 then
   repeat
     if mx[x,y]=1 then begin
       e:=x;
       x:=0;
     end;
     Dec(x);
     Inc(y);
   until (x<0) or (y>(fs.IntValue-1));
   if e>=0 then
     for x:=xa-1 downto e+1 do begin
      y:=ya+(xa-x);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  e:=-1;
  //Sec Diag Begin
  if (ya>1) and (xa<(fs.IntValue-2)) then begin
    /////////////
   x:=xa+1;
   y:=ya-1;
   if mx[x,y]=2 then
   repeat
     if mx[x,y]=1 then begin
       e:=x;
       x:=fs.IntValue-1;
     end;
     Inc(x);
     Dec(y);
   until (y<0) or (x>(fs.IntValue-1));
   if e>=0 then
     for x:=xa+1 to e-1 do begin
      y:=ya+(xa-x);
      SetLength(arr,Length(arr)+1);
      arr[length(arr)-1].x:=x;
      arr[length(arr)-1].y:=y;
     end;
    /////////////
  end;
  //Final
  if length(arr)>0 then begin
   for e:=0 to length(arr)-1 do mx[arr[e].x,arr[e].y]:=1;
   result:=true;
  end else begin
    mx[xa,ya]:=0;
    result:=false;
  end;
end;

procedure TestScore(My,PC: byte);
var s: string;
begin
  if ((my-pc)>(hi1-hi2)) then begin
    s:=InputBox('Новый рекорд','Введите ваше имя:','Безымянный');
    Form1.Label5.Caption:=s;
    hi1:=my;
    hi2:=pc;
    Form1.Label6.Caption:=IntToStr(hi1)+' : '+IntToStr(hi2);
  end;
end;

procedure TForm1.PCA;
var r,n,ttl: byte;
    ok: boolean;
    arr: array[0..15,0..15] of boolean;
begin
  ab:=false;
  Randomize;
  ok:=false;
  repeat
    r:=Random(fs.IntValue);
    n:=Random(fs.IntValue);
    arr[r,n]:=true;
    if mx[r,n]=0 then begin
      mx[r,n]:=1;
      if TestPC(r,n) then ok:=true
       else mx[r,n]:=0;
    end;
    Application.ProcessMessages;
    if ab then begin
      ok:=true;
      ab:=false;
    end;
    ttl:=0;
    for r:=0 to fs.IntValue-1 do
    for n:=0 to fs.IntValue-1 do
    if arr[r,n] then inc(ttl);
  until (ok or (ttl=(fs.IntValue*fs.IntValue)));
  Pnt;
  if ttl=(fs.IntValue*fs.IntValue) then begin
    ShowMessage('Больше нет вариантов ходов!'#13'Ваш счет: '+Label1.Caption+' : '+Label2.Caption);
    TestScore(StrToInt(Label1.Caption),StrToInt(Label2.Caption));
    RzToolbarButton2.Click;
  end;
  ttl:=0;
  for r:=0 to fs.IntValue-1 do
  for n:=0 to fs.IntValue-1 do begin
   if mx[r,n]>0 then inc(ttl);
   if mx[r,n]=0 then begin
    mx[r,n]:=2;
    if not Test(r,n, true) then inc(ttl);
    mx[r,n]:=0;
   end;
 end;
  if ttl=(fs.IntValue*fs.IntValue) then begin
     ShowMessage('Больше нет вариантов ходов!'#13'Ваш счет: '+Label1.Caption+' : '+Label2.Caption);
     TestScore(StrToInt(Label1.Caption),StrToInt(Label2.Caption));
     RzToolbarButton2.Click;
  end;
end;

procedure TForm1.Image1MouseDown(Sender: TObject; Button: TMouseButton;
  Shift: TShiftState; X, Y: Integer);
var xa,ya: integer;
    fs: boolean;
begin
  fs:=false;
  xa:= x div 30;
  ya:= y div 30;
  if mx[xa,ya]=0 then begin
    mx[xa,ya]:=2;
    if Test(xa,ya,false) then w2.Play else fs:=true;
  end else fs:=true;
  if fs then w1.play;
  Pnt;
  if not fs then begin
    sleep(200);
    PCA;
  end;
end;

procedure TForm1.RzToolbarButton1Click(Sender: TObject);
begin
ab:=true;
end;

procedure TForm1.RzToolbarButton2Click(Sender: TObject);
var x,y: integer;
begin
  ab:=true;
  for x:=0 to fs.IntValue-1 do
  for y:=0 to fs.IntValue-1 do mx[x,y]:=0;
  mx[fs.IntValue div 2 - 1,fs.IntValue div 2 - 1]:=2;
  mx[fs.IntValue div 2,fs.IntValue div 2]:=2;
  mx[fs.IntValue div 2 - 1,fs.IntValue div 2]:=1;
  mx[fs.IntValue div 2,fs.IntValue div 2 - 1]:=1;
  Image1.Picture.LoadFromFile(ExtractFileDir(ParamStr(0))+'\BKG.BMP');
  il.draw(Image2.Canvas,0,0,0);
  il.draw(Image3.Canvas,0,0,1);
  Pnt;
end;

procedure TForm1.FormClose(Sender: TObject; var Action: TCloseAction);
begin
 HiSave;
end;

procedure TForm1.fsChange(Sender: TObject);
begin
 Image1.Width:=30*fs.IntValue;
 Image1.Height:=30*fs.IntValue;
 if fs.intValue>9 then Width:=Image1.Width+10;
 Height:=Image1.Height+84;
 RzToolbarButton2.Click;
end;

end.

