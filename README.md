# CTD_Project_1

# Inports

import pandas as pd
import glob
import numpy as np
import math
import matplotlib.pyplot as plt
import calendar
import pytz
import pylab
import matplotlib.patches as mpatches
import time
import datetime 
%matplotlib notebook

# Data

CTD_1_S_Data=pd.read_csv('1OregonShelfSurfacePiercingProfilerMooringSummer.csv')
CTD_1_W_Data=pd.read_csv('1OregonShelfSurfacePiercingProfilerMooringWinter.csv')

CTD_2_S_Data=pd.read_csv('2OregonOffshoreCabledShallowProfilerMooringSummerNEW.csv')
CTD_2_W_Data=pd.read_csv('2OregonOffshoreCabledShallowProfilerMooringWinterNEW.csv')

CTD_3_S_Data=pd.read_csv('3OregonOffshoreCabledDeepProfilerMooringSummer.csv')
CTD_3_W_Data=pd.read_csv('3OregonOffshoreCabledDeepProfilerMooringWinter.csv')

CTD_4_S_Data=pd.read_csv('4OregonSlopeBaseShallowProfilerMooringSummer.csv')
CTD_4_W_Data=pd.read_csv('4OregonSlopeBaseShallowProfilerMooringWinter.csv')

CTD_5_S_Data=pd.read_csv('5OregonSlopeBaseDeepProfilerMooringSummer.csv')
CTD_5_W_Data=pd.read_csv('5OregonSlopeBaseDeepProfilerMooringWinter.csv')

CTD_6_S_Data=pd.read_csv('6AxialBaseShallowProfilerMooringSummer.csv')
CTD_6_W_Data=pd.read_csv('6AxialBaseShallowProfilerMooringWinter.csv')

CTD_7_S_Data=pd.read_csv('7AxialBaseDeepProfilerMooringSummer.csv')
CTD_7_W_Data=pd.read_csv('7AxialBaseDeepProfilerMooringWinter.csv')

# Organize Data

#def ntpSecToDatetime(ntp_seconds,ntp_delta):
    
#timestamp = datetime.datetime.utcfromtimestamp(ntp_seconds - ntp_delta).replace(microsecond=0)
#utc_timezone = pytz.timezone("UTC")
#pst_timezone = pytz.timezone("US/Pacific")
#timeDate = utc_timezone.localize(timestamp).astimezone(pst_timezone)
#timeSec = calendar.timegm(timeDate.timetuple())
#return timeSec

#def ntp_seconds_to_datetime(ntp_seconds,ntp_delta):
#return datetime.datetime.fromtimestamp(ntp_seconds-ntp_delta).replace(microsecond=0).strftime("%I:%M:%S:")

def organizeData(data):
    
    #time=data.time
    #ntp_epoch = datetime.datetime(1970, 1, 1)
    #unix_epoch = datetime.datetime(1972, 1, 1)
    #ntp_delta = (unix_epoch - ntp_epoch).total_seconds()
    
    #convTime = []
    
    #for i in range(0, len(time)):
    #convTime.insert(i, ntp_seconds_to_datetime(ntpSecToDatetime(time[i],ntp_delta),ntp_delta))
    #i += 1
 P=data.pressure
    T=data.temp
    S=data.salinity
    T2 = T*T
    T3 = T*T*T
    Speed=(1449.2+(4.6*T)-(.055*T2)+(.00029*T3)+((1.34-0.01*T)*(S-35))+(.016*P))
    
    temp=[data.time,data.pressure,data.temp,data.salinity,Speed]   
    return temp

print("1")
CTD_1_S=organizeData(CTD_1_S_Data)
CTD_1_W=organizeData(CTD_1_W_Data)
print("2")
CTD_2_S=organizeData(CTD_2_S_Data)
CTD_2_W=organizeData(CTD_2_W_Data)
print("3")
CTD_3_S=organizeData(CTD_3_S_Data)
CTD_3_W=organizeData(CTD_3_W_Data)
print("4")
CTD_4_S=organizeData(CTD_4_S_Data)
CTD_4_W=organizeData(CTD_4_W_Data)
print("5")
CTD_5_S=organizeData(CTD_5_S_Data)
CTD_5_W=organizeData(CTD_5_W_Data)
print("6")
CTD_6_S=organizeData(CTD_6_S_Data)
CTD_6_W=organizeData(CTD_6_W_Data)
print("7")
CTD_7_S=organizeData(CTD_7_S_Data)
CTD_7_W=organizeData(CTD_7_W_Data)

# Plot Dives

def plotDives (data, name, n):
    plt.figure(n)
    plt.title(name +": Depth vs Time")
    plt.xlabel('Time') 
    plt.ylabel('Depth (m)') 
    plt.gca().invert_yaxis()
    plt.plot(data[0],data[1], '.b')
    blue_patch = mpatches.Patch(color='blue', label='ssp')
    plt.legend(handles=[ blue_patch])
    plt.show()
    
plotDives(CTD_1_S,"CTD 1 Summer",1)
plotDives(CTD_1_W,"CTD 1 Winter",2)

plotDives(CTD_2_S,"CTD 2 Summer",3)
plotDives(CTD_2_W,"CTD 2 Winter",4)

plotDives(CTD_3_S,"CTD 3 Summer",5)
plotDives(CTD_3_W,"CTD 3 Winter",6)

plotDives(CTD_4_S,"CTD 4 Summer",7)
plotDives(CTD_4_W,"CTD 4 Winter",8)


plotDives(CTD_5_S,"CTD 5 Summer",9)
plotDives(CTD_5_W,"CTD 5 Winter",10)

plotDives(CTD_6_S,"CTD 6 Summer",11)
plotDives(CTD_6_W,"CTD 6 Summer",12)

plotDives(CTD_7_S,"CTD 7 Summer",13)
plotDives(CTD_7_W,"CTD 7 Winter",14)

# Number of Dives

def getNumberOfDives(data, name):
    
    depth=data[1]
    
    bottom=max(depth)
    atBottom=False
    diveCount=0
    
    for i in range(0,len(depth)):
        if(depth[i] > .95 *bottom and not atBottom):
            atBottom=True
        if(depth[i] < .95 *bottom and atBottom):
            atBottom=False
            diveCount+=1
    print(name +":",diveCount)
    return diveCount

CTD_1_S_Dives = getNumberOfDives(CTD_1_S, "CTD 1 Summer")
CTD_1_W_Dives = getNumberOfDives(CTD_1_W, "CTD 1 Winter")

CTD_2_S_Dives = getNumberOfDives(CTD_2_S, "CTD 2 Summer")
CTD_2_W_Dives = getNumberOfDives(CTD_2_W, "CTD 2 Winter")

CTD_3_S_Dives = getNumberOfDives(CTD_3_S, "CTD 3 Summer")
CTD_3_W_Dives = getNumberOfDives(CTD_3_W, "CTD 3 Winter")

CTD_4_S_Dives = getNumberOfDives(CTD_4_S, "CTD 4 Summer")
CTD_4_W_Dives = getNumberOfDives(CTD_4_W, "CTD 4 Winter")

CTD_5_S_Dives = getNumberOfDives(CTD_5_S, "CTD 5 Summer")
CTD_5_W_Dives = getNumberOfDives(CTD_5_W, "CTD 5 Winter")

CTD_6_S_Dives = getNumberOfDives(CTD_6_S, "CTD 6 Summer")
CTD_6_W_Dives = getNumberOfDives(CTD_6_W, "CTD 6 Winter")

CTD_7_S_Dives = getNumberOfDives(CTD_7_S, "CTD 7 Summer")
CTD_7_W_Dives = getNumberOfDives(CTD_7_W, "CTD 7 Winter")

def AvgData(Data):
    
    Depth=Data[1]
    Speed=Data[4]
    Bottom=max(Depth)
    Output = []
    
    for i in range(0,100):
        Output.append([0.0,0.0,0])
        
    print(len(Depth))
    for i in range(0,len(Depth)):

        for j in range(0,len(Output)):
            
            if(Depth[i] > (j/100)*Bottom and Depth[i]<((j/100)+.01)*Bottom):
                Output[j][0]= Output[j][0]+Depth[i]
                Output[j][1]= Output[j][1]+Speed[i]
                Output[j][2]+=1
                break
    
    OutputDepth = []
    OutputSpeed = []
    
    for i in range(0,len(Output)):
        if (Output[i][2]!=0):
            OutputDepth.append(Output[i][0]/Output[i][2])
            OutputSpeed.append(Output[i][1]/Output[i][2])
            
    return OutputDepth , OutputSpeed
    
    def AvgDatatest(Data):
    Depth=Data[1]
    Speed=Data[4]
    Bottom=max(Depth)
    Output = []
    for i in range(0,25):
        Output.append([0.0,0.0,0])  
    print(len(Depth))
    for i in range(0,len(Depth)):
        for j in range(0,len(Output)):
            if(Depth[i] > (j/25)*Bottom and Depth[i]<((j/25)+.04)*Bottom):
                Output[j][0]= Output[j][0]+Depth[i]
                Output[j][1]= Output[j][1]+Speed[i]
                Output[j][2]+=1
                break
    OutputDepth = []
    OutputSpeed = []
    for i in range(0,len(Output)):
        if (Output[i][2]!=0):
            OutputDepth.append(Output[i][0]/Output[i][2])
            OutputSpeed.append(Output[i][1]/Output[i][2])
    return OutputDepth , OutputSpeed
    
    
    def AvgDataPlot (Data,name, n):
    plt.figure(n)
    plt.scatter(Data[4],Data[1])
    plt.gca().invert_yaxis()
    plt.title(name+": Depth Vs Speed of Sound")
    plt.xlabel('Speed of Sound (m/s)')
    plt.ylabel('Depth (m)')
    
    [avgDepth,avgSpeed]=AvgDatatest(Data)
    
    
    plt.plot(avgSpeed,avgDepth,'r')
    

    red_patch = mpatches.Patch(color='red', label='Average')
    blue_patch = mpatches.Patch(color='blue', label='ssp')
    plt.legend(handles=[red_patch, blue_patch])
    
    print("1")
AvgDataPlot(CTD_1_S,"CTD 1 Summer",15)
AvgDataPlot(CTD_1_W,"CTD 1 Winter",16)
print("2")
AvgDataPlot(CTD_2_S,"CTD 2 Summer",17)
AvgDataPlot(CTD_2_W,"CTD 2 Winter",18)
print("3")
AvgDataPlot(CTD_3_S,"CTD 3 Summer",19)
AvgDataPlot(CTD_3_W,"CTD 3 Winter",20)
print("4")
AvgDataPlot(CTD_4_S,"CTD 4 Summer",21)
AvgDataPlot(CTD_4_W,"CTD 4 Winter",22)
print("5")
AvgDataPlot(CTD_5_S,"CTD 5 Summer",23)
AvgDataPlot(CTD_5_W,"CTD 5 Winter",24)
print("6")
AvgDataPlot(CTD_6_S,"CTD 6 Summer",25)
AvgDataPlot(CTD_6_W,"CTD 6 Winter",26)
print("7")
AvgDataPlot(CTD_7_S,"CTD 7 Summer",27)
AvgDataPlot(CTD_7_W,"CTD 7 Winter",28)
print("out")

maxSummer_temp= max(CTD_1_S[4]),max(CTD_2_S[4]),max(CTD_3_S[4]),max(CTD_4_S[4]),max(CTD_5_S[4]),max(CTD_6_S[4]),max(CTD_7_S[4])
print(maxSummer_temp)
maxSummer= max(maxSummer_temp)
print(maxSummer)
print()
maxWinter_temp= max(CTD_1_W[4]),max(CTD_2_W[4]),max(CTD_3_W[4]),max(CTD_4_W[4]),max(CTD_5_W[4]),max(CTD_6_W[4]),max(CTD_7_W[4])
print(maxWinter_temp)
maxWinter= max(maxWinter_temp)
print(maxWinter)

def plotDayandNight (data, name, n):
    
    plt.figure(n)
    plt.title(name +": Speed of Sound vs Time")
    plt.xlabel('Time') 
    plt.ylabel('Speed of Sound (m/s)') 
    plt.plot(data[0],data[4], '.b')
    blue_patch = mpatches.Patch(color='blue', label='ssp')
    plt.legend(handles=[ blue_patch])
    plt.show()
    
    CTD_1_S_Dives = plotDayandNight(CTD_1_S, "CTD 1 Summer",40)
CTD_1_W_Dives = plotDayandNight(CTD_1_W, "CTD 1 Winter",41)

CTD_2_S_Dives = plotDayandNight(CTD_2_S, "CTD 2 Summer",42)
CTD_2_W_Dives = plotDayandNight(CTD_2_W, "CTD 2 Winter",43)

CTD_3_S_Dives = plotDayandNight(CTD_3_S, "CTD 3 Summer",44)
CTD_3_W_Dives = plotDayandNight(CTD_3_W, "CTD 3 Winter",45)

CTD_4_S_Dives = plotDayandNight(CTD_4_S, "CTD 4 Summer",46)
CTD_4_W_Dives = plotDayandNight(CTD_4_W, "CTD 4 Winter",47)

CTD_5_S_Dives = plotDayandNight(CTD_5_S, "CTD 5 Summer",48)
CTD_5_W_Dives = plotDayandNight(CTD_5_W, "CTD 5 Winter",49)

CTD_6_S_Dives = plotDayandNight(CTD_6_S, "CTD 6 Summer",50)
CTD_6_W_Dives = plotDayandNight(CTD_6_W, "CTD 6 Winter",51)

CTD_7_S_Dives = plotDayandNight(CTD_7_S, "CTD 7 Summer",52)
CTD_7_W_Dives = plotDayandNight(CTD_7_W, "CTD 7 Winter",53)


plt.figure(35)
plt.gca().invert_yaxis()
plt.plot(CTD_1_S[4],CTD_1_S[1],'pink')
plt.plot(CTD_2_S[4],CTD_2_S[1],'green')
plt.plot(CTD_3_S[4],CTD_3_S[1],'blue')
plt.plot(CTD_4_S[4],CTD_4_S[1],'red')
plt.plot(CTD_5_S[4],CTD_5_S[1],'black')
plt.plot(CTD_6_S[4],CTD_6_S[1],'purple')
plt.plot(CTD_7_S[4],CTD_7_S[1],'magenta')
plt.title("Summer: Depth Vs Speed of Sound")
plt.xlabel('Speed of Sound (m/s)')
plt.ylabel('Depth (m)')
pk = mpatches.Patch(color='pink', label='CTD_1_S')
g = mpatches.Patch(color='green', label='CTD_2_S')
b = mpatches.Patch(color='blue', label='CTD_3_S')
r = mpatches.Patch(color='red', label='CTD_4_S')
bl = mpatches.Patch(color='black', label='CTD_5_S')
p = mpatches.Patch(color='purple', label='CTD_6_S')
m = mpatches.Patch(color='magenta', label='CTD_7_S')
plt.legend(handles=[pk,g,b,r,bl,p,m])


plt.figure(36)
plt.gca().invert_yaxis()
plt.plot(CTD_1_W[4],CTD_1_W[1],'pink')
plt.plot(CTD_2_W[4],CTD_2_W[1],'green')
plt.plot(CTD_3_W[4],CTD_3_W[1],'blue')
plt.plot(CTD_4_W[4],CTD_4_W[1],'red')
plt.plot(CTD_5_W[4],CTD_5_W[1],'black')
plt.plot(CTD_6_W[4],CTD_6_W[1],'purple')
plt.plot(CTD_7_W[4],CTD_7_W[1],'magenta')
plt.title("Winter: Depth Vs Speed of Sound")
plt.xlabel('Speed of Sound (m/s)')
plt.ylabel('Depth (m)')
pk = mpatches.Patch(color='pink', label='CTD_1_S')
g = mpatches.Patch(color='green', label='CTD_2_S')
b = mpatches.Patch(color='blue', label='CTD_3_S')
r = mpatches.Patch(color='red', label='CTD_4_S')
bl = mpatches.Patch(color='black', label='CTD_5_S')
p = mpatches.Patch(color='purple', label='CTD_6_S')
m = mpatches.Patch(color='magenta', label='CTD_7_S')
plt.legend(handles=[pk,g,b,r,bl,p,m])
