module alu(
    input  wire [2:0] op,
    input  wire [7:0] imm,    // will be zero-extended during decoding
    input  wire [7:0] acc, 
    output reg  [7:0] alu_out
);

    always @(*) begin
        case (op)
            3'b000:  alu_out = imm;        // LOAD instruction
            3'b001:  alu_out = acc + imm; 
            3'b010:  alu_out = acc - imm; 
            default: alu_out = acc;        
        endcase
    end

endmodule