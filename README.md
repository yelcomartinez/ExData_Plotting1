## Introduction

This assignment uses data from
the <a href="http://archive.ics.uci.edu/ml/">UC Irvine Machine
Learning Repository</a>, a popular repository for machine learning
datasets. In particular, we will be using the "Individual household
electric power consumption Data Set" which I have made available on
the course web site:


* <b>Dataset</b>: <a href="https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2Fhousehold_power_consumption.zip">Electric power consumption</a> [20Mb]

* <b>Description</b>: Measurements of electric power consumption in
one household with a one-minute sampling rate over a period of almost
4 years. Different electrical quantities and some sub-metering values
are available.


The following descriptions of the 9 variables in the dataset are taken
from
the <a href="https://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption">UCI
web site</a>:

<ol>
<li><b>Date</b>: Date in format dd/mm/yyyy </li>
<li><b>Time</b>: time in format hh:mm:ss </li>
<li><b>Global_active_power</b>: household global minute-averaged active power (in kilowatt) </li>
<li><b>Global_reactive_power</b>: household global minute-averaged reactive power (in kilowatt) </li>
<li><b>Voltage</b>: minute-averaged voltage (in volt) </li>
<li><b>Global_intensity</b>: household global minute-averaged current intensity (in ampere) </li>
<li><b>Sub_metering_1</b>: energy sub-metering No. 1 (in watt-hour of active energy). It corresponds to the kitchen, containing mainly a dishwasher, an oven and a microwave (hot plates are not electric but gas powered). </li>
<li><b>Sub_metering_2</b>: energy sub-metering No. 2 (in watt-hour of active energy). It corresponds to the laundry room, containing a washing-machine, a tumble-drier, a refrigerator and a light. </li>
<li><b>Sub_metering_3</b>: energy sub-metering No. 3 (in watt-hour of active energy). It corresponds to an electric water-heater and an air-conditioner.</li>
</ol>

## Code


######################PLOT 1######################
#Read data
PW <- data.table::fread(input = "household_power_consumption.txt", na.strings="?")

# Put Date Type
PW[, Date := lapply(.SD, as.Date, "%d/%m/%Y"), .SDcols = c("Date")]

# Dates from 2007-02-01 to 2007-02-02
PWT <- PW[(Date >= "2007-02-01") & (Date <= "2007-02-02")]

png("plot1.png", width=480, height=480)

## Plot 1
hist(PW[, Global_active_power], main="Global Active Power", 
     xlab="Global Active Power (kilowatts)", ylab="Frequency", col="Red")

dev.off()

######################PLOT 2######################

#Read data
PW <- data.table::fread(input = "household_power_consumption.txt", na.strings="?")

# Making data readable
PW[, dateTime := as.POSIXct(paste(Date, Time), format = "%d/%m/%Y %H:%M:%S")]

# Dates from 2007-02-01 to 2007-02-02
PW <- PW[(dateTime >= "2007-02-01") & (dateTime < "2007-02-03")]

png("plot2.png", width=480, height=480)

## Plot 2
plot(x = PW[, dateTime]
     , y = PW[, Global_active_power]
     , type="l", xlab="", ylab="Global Active Power (kilowatts)")

dev.off()

######################PLOT 3######################

#Read data
PW <- data.table::fread(input = "household_power_consumption.txt", na.strings="?")

# Making data readable
PW[, dateTime := as.POSIXct(paste(Date, Time), format = "%d/%m/%Y %H:%M:%S")]

# Data from 2007-02-01 to 2007-02-02
PW <- PW[(dateTime >= "2007-02-01") & (dateTime < "2007-02-03")]

png("plot3.png", width=480, height=480)

# Plot 3
plot(PW[, dateTime], PW[, Sub_metering_1], type="l", xlab="", ylab="Energy sub metering")
lines(PW[, dateTime], PW[, Sub_metering_2],col="red")
lines(PW[, dateTime], PW[, Sub_metering_3],col="blue")
legend("topright"
       , col=c("black","red","blue")
       , c("Sub_metering_1  ","Sub_metering_2  ", "Sub_metering_3  ")
       ,lty=c(1,1), lwd=c(1,1))

dev.off()

######################PLOT 4######################

#Read data
PW <- data.table::fread(input = "household_power_consumption.txt", na.strings="?")

# Making data readable
PW[, dateTime := as.POSIXct(paste(Date, Time), format = "%d/%m/%Y %H:%M:%S")]

# Data from 2007-02-01 to 2007-02-02
PW <- PW[(dateTime >= "2007-02-01") & (dateTime < "2007-02-03")]

png("plot4.png", width=480, height=480)

par(mfrow=c(2,2))

# Plot 1
plot(PW[, dateTime], PW[, Global_active_power], type="l", xlab="", ylab="Global Active Power")

# Plot 2
plot(PW[, dateTime],PW[, Voltage], type="l", xlab="datetime", ylab="Voltage")

# Plot 3
plot(PW[, dateTime], PW[, Sub_metering_1], type="l", xlab="", ylab="Energy sub metering")
lines(PW[, dateTime], PW[, Sub_metering_2], col="red")
lines(PW[, dateTime], PW[, Sub_metering_3],col="blue")
legend("topright", col=c("black","red","blue")
       , c("Sub_metering_1  ","Sub_metering_2  ", "Sub_metering_3  ")
       , lty=c(1,1)
       , bty="n"
       , cex=.5) 

# Plot 4
plot(PW[, dateTime], PW[,Global_reactive_power], type="l", xlab="datetime", ylab="Global_reactive_power")

dev.off()



