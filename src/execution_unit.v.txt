module execution_unit (
    input wire clk,
    input wire rst_n,
    input wire run,
    input wire HALT,
    output wire exec,
    output reg halted
);

    assign exec = run & ~halted;

    always @(posedge clk) begin
        if (!rst_n)
            halted <= 1'b0;
        else if (exec && HALT)
            halted <= 1'b1;
    end

endmodule
