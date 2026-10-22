module program_loader (
	input wire clk,
	input wire rst_n,
	input wire prog_load,
	input wire run, // run = 1 cpu still busy, don't load anything
	output reg [2:0] load_addr, //the instruction fed to the program memory
	output wire load_pulse	//serves as a write enable signal to your program memory
);
	
	reg pl_q;

	assign load_pulse = prog_load & ~pl_q & !run; //refer to table 4.2 

	always @(posedge clk) begin
		pl_q <= prog_load; //just states the previous value of prog load
		if (!rst_n) //remember the reset is active-low !!
			load_addr <= 3'b0;
		else if (load_pulse) begin
			load_addr <= load_addr + 3'd1; //onto the next instruction
		end
	end
endmodule