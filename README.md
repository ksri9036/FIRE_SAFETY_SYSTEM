# FIRE_SAFETY_SYSTEM
# Abstract 
This project focuses on designing an automated fire safety monitoring system using Verilog Hardware Description Language (HDL) targeted for commercial and industrial applications. The system simulates smoke and temperature sensors to continuously monitor environmental conditions and detect hazardous thresholds. Upon detecting smoke or high temperature beyond preset limits, the system triggers alarms and activates sprinklers automatically through a finite state machine (FSM) control sequence. The design employs simple conditional logic and combinational comparators to determine alert conditions, an FSM to handle alarm and sprinkler activation states, and display outputs to indicate safe or alert status. This approach ensures timely hazard detection and response with digital control, enhancing safety and reducing human intervention in critical fire emergency situations. The modular structure makes it suitable for implementation on FPGA or ASIC platforms, serving as an efficient and affordable fire detection and safety system. If desired, the design can be extended with real sensor interfacing, additional alert mechanisms, or integration into broader industrial safety networks.

# Introduction 
This project presents a Verilog HDL-based fire safety monitoring system designed for commercial and industrial applications. The system utilizes simulated smoke and temperature sensors and employs conditional logic to compare sensor outputs against preset thresholds. Based on these inputs, a finite state machine controls the activation of alarms and sprinklers, ensuring an automatic and timely response to fire hazards. The system also provides real-time visual status indicators for safe and alert conditions, enhancing situational awareness.
Implemented for FPGA deployment, the system offers a hardware-level solution that aligns with modern fire protection requirements, emphasizing reliability, automation, and quick hazard detection. Its modular design allows integration with real sensors and further expansion to meet industrial safety standards, thereby improving emergency management and protecting life and property efficiently. 
 


# Problem Statement 
The problem addressed by this project is the critical need for an automated fire safety system capable of real-time monitoring and rapid response in commercial and industrial settings. Traditional fire detection methods often rely on manual intervention or systems prone to delays, increasing the risk of severe damage to life and property. Additionally, false alarms and inadequate sensor integration can compromise effectiveness, leading to delayed emergency responses or system neglect.This project seeks to design a reliable, hardware-based fire safety system using Verilog HDL that continuously monitors simulated smoke and temperature levels. By employing conditional logic and a finite state machine, it ensures early detection of hazardous conditions and automatic activation of alarms and sprinklers. The solution aims to reduce human dependency, minimize false alerts, and provide clear status indication, thereby enhancing safety protocols and aligning with industrial fire protection standards for efficient and timely hazard mitigation.

# Block diagram:
 
![block diagram](https://github.com/user-attachments/assets/c1d52deb-8246-4ebc-ba6b-eeb41d4237ae)


# Verilog Code : 
'''
module FireSafetyConditional(
    input clk,
    input reset,
    input [7:0] smoke,
    input [7:0] temp,
    output reg alarm,
    output reg sprinkler,
    output reg safe_indicator,
    output reg alert_indicator
);

parameter SMOKE_THRESHOLD = 8'd100;
parameter TEMP_THRESHOLD = 8'd60;

// State encoded as a reg for simplicity
reg [1:0] state;
parameter SAFE = 2'b00, ALERT = 2'b01, SPRINKLER = 2'b10;

always @(posedge clk or posedge reset) 
begin
    if (reset) 
    begin
        state <= SAFE;
    end 
    else 
    begin
        // Conditional state transitions
        if (state == SAFE) 
        begin
            if ((smoke > SMOKE_THRESHOLD) || (temp > TEMP_THRESHOLD))
                state <= ALERT;
            else
                state <= SAFE;
        end else if (state == ALERT) 
        begin
            if ((smoke > SMOKE_THRESHOLD) || (temp > TEMP_THRESHOLD))
                state <= SPRINKLER;
            else
                state <= SAFE;
        end else if (state == SPRINKLER) 
        begin
            if ((smoke > SMOKE_THRESHOLD) || (temp > TEMP_THRESHOLD))
                state <= SPRINKLER;
            else
                state <= SAFE;
        end 
        else 
        begin
            state <= SAFE;
        end
    end
end

always @(*) begin
    // Conditional output logic
    if (state == SAFE) 
    begin
        alarm = 0;
        sprinkler = 0;
        safe_indicator = 1;
        alert_indicator = 0;
    end 
    else if (state == ALERT) 
    begin
        alarm = 1;
        sprinkler = 0;
        safe_indicator = 0;
        alert_indicator = 1;
    end
    else if (state == SPRINKLER)
    begin
        alarm = 1;
        sprinkler = 1;
        safe_indicator = 0;
        alert_indicator = 1;
    end
    else 
    begin
        alarm = 0;
        sprinkler = 0;
        safe_indicator = 1;
        alert_indicator = 0;
    end
end
endmodule
'''
# TESTBENCH:
'''
module tb_FireSafetyConditional;

// Inputs to DUT
reg clk;
reg reset;
reg [7:0] smoke;
reg [7:0] temp;

// Outputs from DUT
wire alarm;
wire sprinkler;
wire safe_indicator;
wire alert_indicator;

// Instantiate the DUT (Device Under Test)
FireSafetyConditional dut (
    .clk(clk),
    .reset(reset),
    .smoke(smoke),
    .temp(temp),
    .alarm(alarm),
    .sprinkler(sprinkler),
    .safe_indicator(safe_indicator),
    .alert_indicator(alert_indicator)
);

// Clock generation: 10 time unit period
initial clk = 0;
always #5 clk = ~clk;

// Test sequence
initial begin
    // Initialize inputs
    reset = 1;
    smoke = 8'd0;
    temp = 8'd0;
    #10;
    
    // Release reset
    reset = 0;
    #10;
    
    // Test case 1: SAFE state (below thresholds)
    smoke = 8'd50;
    temp = 8'd40;
    #20;
    
    // Test case 2: ALERT state (temp above threshold)
    temp = 8'd70;
    smoke = 8'd50;
    #20;
    
    // Test case 3: SPRINKLER state (both sensors above threshold)
    temp = 8'd70;
    smoke = 8'd110;
    #20;
    
    // Test case 4: Return to SAFE state (both sensors below threshold)
    temp = 8'd40;
    smoke = 8'd50;
    #20;
    
    // End simulation
    $stop;
end
endmodule 
'''
# Simulation Results :  

<img width="1920" height="1200" alt="Screenshot 2025-11-18 192006" src="https://github.com/user-attachments/assets/f15ae719-3e07-4f19-bbb1-5cfb1b427f1a" />


# Conclusion 
In conclusion, the Verilog-based fire safety monitoring system developed in this project offers an effective and reliable solution for early fire detection and automated hazard response in industrial and commercial environments. By integrating simulated smoke and temperature sensors with comparator logic and a finite state machine, the system ensures precise monitoring and rapid activation of alarms and sprinklers. This automation minimizes human intervention, reduces response delays, and provides clear status indications, enhancing overall safety. The system's modular design and FPGA-compatible implementation enable scalability and adaptability for real-world deployment, aligning with modern fire protection standards. Implementing such intelligent fire safety systems delivers key benefits including early warning, property protection, life safety, regulatory compliance, and significant risk mitigation, ultimately safeguarding lives and assets while optimizing emergency response efficiency.
In conclusion, the Verilog-based fire safety monitoring system developed in this project offers an effective and reliable solution for early fire detection and automated hazard response in industrial and commercial environments. By integrating simulated smoke and temperature sensors with comparator logic and a finite state machine, the system ensures precise monitoring and rapid activation of alarms and sprinklers. This automation minimizes human intervention, reduces response delays, and provides clear status indications, enhancing overall safety. The system's modular design and FPGA-compatible implementation enable scalability and adaptability for real-world deployment, aligning with modern fire protection standards. Implementing such intelligent fire safety systems delivers key benefits including early warning, property protection, life safety, regulatory compliance, and significant risk mitigation, ultimately safeguarding lives and assets while optimizing emergency response efficiency.
