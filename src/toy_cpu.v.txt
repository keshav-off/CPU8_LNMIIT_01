module toy_cpu (
    input wire clk,
    input wire rst_n,
    input wire [7:0] prog_data,
    input wire prog_load,
    input wire run,
    output wire [7:0] out_data
);

    wire [2:0] pc;
    wire halted;
    reg [7:0] acc;
    reg [7:0] ir;
    reg [7:0] out_r;

    wire [2:0] load_addr;
    wire load_pulse;
    wire [7:0] instr;
    wire [2:0] op;
    wire [4:0] imm;
    wire [2:0] target;
    wire HALT;
    wire [7:0] alu_out;
    wire zero_flag;
    wire exec;

    program_loader u_loader (
        .clk(clk),
        .rst_n(rst_n),
        .prog_load(prog_load),
        .run(run),
        .load_addr(load_addr),
        .load_pulse(load_pulse)
    );

    program_memory u_mem (
        .clk(clk),
        .load_addr(load_addr),
        .pc(pc),
        .prog_data(prog_data),
        .load_pulse(load_pulse & rst_n),
        .instr(instr)
    );

    decode_unit u_decode (
        .clk(clk),
        .instr(instr),
        .op(op),
        .imm(imm),
        .target(target),
        .HALT(HALT)
    );

    alu u_alu (
        .op(op),
        .imm(imm),
        .acc(acc),
        .alu_out(alu_out),
        .zero_flag(zero_flag)
    );

    execution_unit u_exec (
        .clk(clk),
        .rst_n(rst_n),
        .run(run),
        .HALT(HALT),
        .exec(exec),
        .halted(halted)
    );

    program_counter u_pc (
        .clk(clk),
        .rst_n(rst_n),
        .exec(exec),
        .op(op),
        .target(target),
        .zero_flag(zero_flag),
        .pc(pc)
    );

    always @(posedge clk) begin
        if (!rst_n) begin //reset all values when reset is set to a zero
            acc <= 8'h00;
            ir <= 8'h00;
            out_r <= 8'h00;
        end else if (exec) begin 
            ir <= instr; //storing the instruction being executed
            acc <= alu_out;
            if (op == 3'b011 || op == 3'b110) //store and acc are the same operation as mentioned in the file
                out_r <= acc;
        end
    end

    assign out_data = out_r; //output value 

endmodule
