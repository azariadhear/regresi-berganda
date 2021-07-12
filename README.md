# regresi-berganda
function varargout = regresi_berganda(varargin)
% REGRESI_BERGANDA MATLAB code for regresi_berganda.fig
%      REGRESI_BERGANDA, by itself, creates a new REGRESI_BERGANDA or raises the existing
%      singleton*.
%
%      H = REGRESI_BERGANDA returns the handle to a new REGRESI_BERGANDA or the handle to
%      the existing singleton*.
%
%      REGRESI_BERGANDA('CALLBACK',hObject,eventData,handles,...) calls the local
%      function named CALLBACK in REGRESI_BERGANDA.M with the given input arguments.
%
%      REGRESI_BERGANDA('Property','Value',...) creates a new REGRESI_BERGANDA or raises the
%      existing singleton*.  Starting from the left, property value pairs are
%      applied to the GUI before regresi_berganda_OpeningFcn gets called.  An
%      unrecognized property name or invalid value makes property application
%      stop.  All inputs are passed to regresi_berganda_OpeningFcn via varargin.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help regresi_berganda

% Last Modified by GUIDE v2.5 12-Jun-2021 19:42:31

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @regresi_berganda_OpeningFcn, ...
                   'gui_OutputFcn',  @regresi_berganda_OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT


% --- Executes just before regresi_berganda is made visible.
function regresi_berganda_OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   command line arguments to regresi_berganda (see VARARGIN)

% Choose default command line output for regresi_berganda
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes regresi_berganda wait for user response (see UIRESUME)
% uiwait(handles.figure1);


% --- Outputs from this function are returned to the command line.
function varargout = regresi_berganda_OutputFcn(hObject, eventdata, handles) 
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;


% --- Executes on button press in data_btn.
function data_btn_Callback(hObject, eventdata, handles)
% hObject    handle to data_btn (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
X = xlsread('data.xlsx'); % Membaca excel dan memasukkan ke variabel X
set(handles.tabel, 'data', X); % Menampilkan data ke tabel GUI Matlab

% --- Executes on button press in grafik_btn.
function grafik_btn_Callback(hObject, eventdata, handles)
% hObject    handle to grafik_btn (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
clc;
axes(handles.grafik)
t = xlsread('data.xlsx');

y = t(:,1); % harga saham (variabel tak bebas)
x1 = t(:,2); % PER (variabel bebas)
x2 = t(:,3); % ROI (variabel bebas)
x = [x1,x2];
mdl = fitlm(x,y)

X = [ones(size(x1)) x1 x2];
b = regress(y,X)
scatter3(x1,x2,y,'filled')
hold on
x1fit = linspace(min(x1),max(x1),15);
x2fit = linspace(min(x2),max(x2),15);
[X1FIT,X2FIT] = meshgrid(x1fit,x2fit);
YFIT = b(1) + b(2)*X1FIT + b(3)*X2FIT;
mesh(X1FIT,X2FIT,YFIT)
xlabel('PER')
ylabel('ROI')
zlabel('Harga Saham')
view(75,5)
hold off
legend({'Data Asli','Data Hasil Regresi'},'Location','southoutside','NumColumns',2)
title('\fontsize{10}\color[rgb]{0 0.45 0.74}Regresi Berganda')
guidata(hObject, handles); %updates the handles

set(handles.beta,'String',b); 

% --- Executes on button press in keluar_btn.
function keluar_btn_Callback(hObject, eventdata, handles)
% hObject    handle to keluar_btn (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
close
