import React, { useState } from "react";

interface WithdrawalSectionProps {
  onWithdraw: (amount: number) => void;
  balance: number;
}

const MIN_WITHDRAWAL = 15;
const MAX_WITHDRAWAL = 100;

const WithdrawalSection: React.FC<WithdrawalSectionProps> = ({
  onWithdraw,
  balance,
}) => {
  const [amount, setAmount] = useState<number>(MIN_WITHDRAWAL);
  const [error, setError] = useState<string>("");

  const handleWithdraw = () => {
    if (amount < MIN_WITHDRAWAL) {
      setError(`Minimum withdrawal amount is ${MIN_WITHDRAWAL}`);
      return;
    }
    if (amount > MAX_WITHDRAWAL) {
      setError(`Maximum withdrawal amount is ${MAX_WITHDRAWAL}`);
      return;
    }
    if (amount > balance) {
      setError(`Insufficient balance`);
      return;
    }
    setError("");
    onWithdraw(amount);
  };

  return (
    <div
      style={{
        border: "1px solid #ddd",
        padding: "20px",
        borderRadius: "8px",
        maxWidth: "350px",
        margin: "0 auto",
        background: "#f9f9f9",
      }}
    >
      <h2>Withdraw</h2>
      <div style={{ marginBottom: "10px" }}>
        <label>
          Enter amount ({MIN_WITHDRAWAL}-{MAX_WITHDRAWAL}):
          <input
            type="number"
            min={MIN_WITHDRAWAL}
            max={MAX_WITHDRAWAL}
            value={amount}
            onChange={(e) => setAmount(Number(e.target.value))}
            style={{ marginLeft: "10px", width: "80px" }}
          />
        </label>
      </div>
      <div style={{ marginBottom: "10px" }}>
        <strong>Balance:</strong> {balance}
      </div>
      {error && (
        <div style={{ color: "red", marginBottom: "10px" }}>{error}</div>
      )}
      <button onClick={handleWithdraw} style={{ padding: "8px 16px" }}>
        Withdraw
      </button>
    </div>
  );
};

export default WithdrawalSection;
