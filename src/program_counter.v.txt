module program_counter (
    input wire clk,
    input wire rst_n,
    input wire exec,
    input wire [2:0] op,
    input wire [2:0] target,
    input wire zero_flag,
    output reg [2:0] pc
);

    reg [2:0] pc_n;

    always @(*) begin
        pc_n = pc + 3'd1;
        case (op)
            3'b100: pc_n = target; //Jump instruction
            3'b101: begin //jump if zero
                if (zero_flag)
                    pc_n = target;
            end
            3'b111: pc_n = pc; //halt 
            default: pc_n = pc + 3'd1;
        endcase
    end

    always @(posedge clk) begin
        if (!rst_n)
            pc <= 3'd0;
        else if (exec)
            pc <= pc_n;
    end

endmodule
