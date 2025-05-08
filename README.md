# evolucao-profissional
import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";
import { Progress } from "@/components/ui/progress";

export default function Dashboard() {
  const [goals, setGoals] = useState([]);
  const [newGoal, setNewGoal] = useState("");
  const [journal, setJournal] = useState([]);
  const [entry, setEntry] = useState("");

  const addGoal = () => {
    if (newGoal.trim() !== "") {
      setGoals([...goals, { text: newGoal, done: false }]);
      setNewGoal("");
    }
  };

  const toggleGoal = (index) => {
    const updated = [...goals];
    updated[index].done = !updated[index].done;
    setGoals(updated);
  };

  const addJournalEntry = () => {
    if (entry.trim() !== "") {
      setJournal([{ date: new Date().toLocaleString(), text: entry }, ...journal]);
      setEntry("");
    }
  };

  const progress = goals.length
    ? (goals.filter((g) => g.done).length / goals.length) * 100
    : 0;

  return (
    <div className="max-w-3xl mx-auto p-6 space-y-6">
      <h1 className="text-3xl font-bold">Painel de Evolução Profissional</h1>
      <Tabs defaultValue="metas" className="space-y-4">
        <TabsList className="grid grid-cols-3 gap-2">
          <TabsTrigger value="metas">Metas</TabsTrigger>
          <TabsTrigger value="diario">Diário</TabsTrigger>
          <TabsTrigger value="progresso">Progresso</TabsTrigger>
        </TabsList>

        <TabsContent value="metas">
          <Card>
            <CardContent className="p-4 space-y-4">
              <h2 className="text-xl font-semibold">Adicionar Nova Meta</h2>
              <div className="flex gap-2">
                <Input
                  placeholder="Ex: Estudar funis de venda"
                  value={newGoal}
                  onChange={(e) => setNewGoal(e.target.value)}
                />
                <Button onClick={addGoal}>Adicionar</Button>
              </div>
              <ul className="space-y-2">
                {goals.map((goal, idx) => (
                  <li
                    key={idx}
                    className={`flex justify-between items-center p-2 rounded border ${goal.done ? "bg-green-100" : "bg-white"}`}
                  >
                    <span className={goal.done ? "line-through" : ""}>{goal.text}</span>
                    <Button onClick={() => toggleGoal(idx)} variant="outline">
                      {goal.done ? "Desfazer" : "Concluir"}
                    </Button>
                  </li>
                ))}
              </ul>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="diario">
          <Card>
            <CardContent className="p-4 space-y-4">
              <h2 className="text-xl font-semibold">Entrada no Diário</h2>
              <Textarea
                placeholder="O que aprendi hoje? Como me senti?"
                value={entry}
                onChange={(e) => setEntry(e.target.value)}
              />
              <Button onClick={addJournalEntry}>Registrar</Button>
              <div className="space-y-4 pt-4">
                {journal.map((item, idx) => (
                  <div key={idx} className="border rounded p-3 bg-gray-50">
                    <div className="text-sm text-gray-500">{item.date}</div>
                    <div>{item.text}</div>
                  </div>
                ))}
              </div>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="progresso">
          <Card>
            <CardContent className="p-4 space-y-4">
              <h2 className="text-xl font-semibold">Progresso Geral</h2>
              <Progress value={progress} />
              <div>{progress.toFixed(1)}% das metas concluídas</div>
            </CardContent>
          </Card>
        </TabsContent>
      </Tabs>
    </div>
  );
}
