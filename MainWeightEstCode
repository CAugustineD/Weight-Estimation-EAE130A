# Weight-Estimation-EAE130A
##Preliminary Weight Estimation for Agricultural Aircraft
import numpy as np
import matplotlib.pyplot as plt


#Step 1: Establish Requirements 
## Payload Profile:
# Total Pilots = 1, pilot weight = 180 Lbs (81.65 Kg)
# Total Passengers = 0
# Total Cargo/Pesticides (pounds) = 100 Lbs (45.36 Kg)

#Step 2: Obtain Historical Data and Step 3: List Assumptions
## Flight Profile:
#Range = 200 miles (321.87 Km)
#Endurance = 120 minutes 
#Specific Fuel Consumption = 0.5 lb/hp/hr (0.305 Kg/kW/hr)
#Crusing Speed = 100 mph (160.93 Km/hr)
#(L/D)max = 10 
#L/D = 0.94 * (L/D)max 

#Step 4: Compute Payload Weight (humans + luggage + cargo)

num_pilot = 1
num_crew = 0
num_crew = num_pilot + num_crew
num_passengers = 0
num_pesticides = 2000 #Lbs (45.36 Kg)
avg_wt_person = 180 #Lbs (81.65 Kg)
avg_wt_luggage = 60 #Lbs (27.22 Kg)

W_crew = num_crew * (avg_wt_person + avg_wt_luggage) #Lbs
print("w_crew: " +str(W_crew) + "Lbs")

W_payload = num_pesticides + num_passengers * (avg_wt_person + avg_wt_luggage) #Lbs
print("W_payload: " +str(W_payload) + "Lbs")

Total_payload= W_crew + W_payload
print("Total Payload: " +str(Total_payload) + "Lbs")



#Step 5: Estimate Fuel Fractions for Cruise and Loiter
L_D_max = 10
L_D = 0.94 * L_D_max
R = 200 #miles 
E = 120/60 #minutes to hours
c = 0.52 #lb/(lbf-hr)
V = 100*1.94 #mph to knots

W1_W0 = 0.996 #takeoff
print("Takeoff Fuel Fraction (W1/W0): " +str(W1_W0))

W2_W1 = 0.998 #climb
print("Climb Fuel Fraction (W2/W1): " +str(W2_W1))

W3_W2 = np.exp((-R*c)/(V*L_D)) #cruise
print("cruise Fuel Fraction (W3/W2): " +str(round(W3_W2, 3)))

W4_W3 = np.exp((-E*c)/(L_D)) #loiter/descent
print("Loiter Fuel Fraction (W4/W3): " +str(round(W4_W3, 3)))

W5_W4 = 0.998 #Landing
print("Landing Fuel Fraction (W5/W4): " +str(W5_W4)) 

#Step 5 Continue: Estimating the Final Fuel Fraction

W5_W0 = W5_W4*W4_W3*W3_W2*W2_W1*W1_W0 
print("Final fuel fraction (W5/W0): " +str(round(W5_W0, 3)))

Wf_W0 = (1-W5_W0)*1.06 #Total fuel fraction with 6% for reserves and trapped fuel
print("Total fuel fraction with reserves (Wf/W0): " +str(round(Wf_W0, 3)))

#Step 6: Iterative Weight Estimation With Added Battery Weight for Hybrid Design
W0 = 8000 #Initial Guess lbs
W0_history = [] #List of initial guesses
err = 1e-6 #tolerance
delta = 2*err #for any value greater than err

eta = 0.7 #70% efficiency
e = 317.515*3600 #Wh/lb to Ws/lb
A = 2.36
C = -0.18
g = 32.2 #ft/s^2 (9.81 m/s^2)

while delta > err:
    W0_history.append(W0)                                 # add latest value to list
    We_W0 = A*W0**C                                       # lbs, Compute empty weight ratio
    
    #W0_new = (W_crew + W_payload)/(1-Wf_W0-We_W0) #Without battery weight
    W0_new = (W_crew + W_payload) / (1 - Wf_W0 -We_W0 - ((R * g) / (eta * e * L_D)))   #lbs, compute new TOGW Hybrid weight
    delta = abs(W0_new - W0) / abs(W0_new)                # find difference between last guess and current guess  
    W0 = W0_new                                           # lbs, update TOGW value
  
W0_history = np.array(W0_history)
#Plot Convergence
plt.figure(figsize=(8,4))
plt.title('Weight Estimate Convergence')
plt.xlabel("Iteration")
plt.ylabel("W0 (Lbs)")
plt.plot(W0_history, label = "W0", linestyle = '-', linewidth = 2, marker = None, markersize = 8)
plt.grid(True)
plt.legend
plt.show()

We = We_W0

m_batt = (R*W0)/(eta*e*L_D)
We = We_W0 *W0
W_hybrid = W0 - W_crew - W_payload -We
print("Takeoff Gross Weight: " + str(round(W0)) + " lbs")
print("Empty Weight: " + str(round(We)) + " lbs")
print("Empty Weight Fraction: {:.3f}".format(We_W0))
print("Battery & Motor Weight: " + str(round(W_hybrid)) + " lbs")
print("Battery & Motor Weight Fraction: {:.3f}".format(W_hybrid/W0))

print("Empty Weight: " +str(round(We))+ "lbs")
print("Our regression's Empty Weight Fraction (we/W0): " +str(round(We_W0, 3)))
print ("Takeoff Gross Weight: " + str(round(W0)) +"lbs")

    


