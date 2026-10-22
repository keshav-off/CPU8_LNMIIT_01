module program_memory(
	input clk,
	input wire [2:0] load_addr,
	input wire [2:0] pc, //keeps track of current instruction
	input wire [7:0] prog_data, //the input instruction that needs storing
	input load_pulse,
	output wire [7:0] instr
);

reg [7:0] mem [0:7]; // a clean 8-bit 8 word register!

	always @(posedge clk) begin
		if (load_pulse) begin
			mem[load_addr] <= prog_data; //load input data only if load pulse is high
		end
	end

assign instr = mem[pc];	
endmodule