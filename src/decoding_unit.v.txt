module decode_unit(
	input wire clk,
	input wire [7:0] instr,
	output wire [2:0] op, //to be fed into the ALU
	output wire [4:0] imm,
	output wire [2:0] target,
	output wire HALT 
);

	assign op = instr[7:5];
	assign imm = instr[4:0];
	assign target = instr[2:0];
	assign HALT = (opcode == 3'b111);
endmodule