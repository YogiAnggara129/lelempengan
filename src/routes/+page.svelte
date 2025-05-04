<script lang="ts">
  import { onMount } from 'svelte';
  import { writable, derived } from 'svelte/store';

  type Player = 'B' | 'R';
  type Cell = Player | '';
  type Board = Cell[][];

  const size = 3;

  const initialBoard: Board = [
    ['B', '', 'R'],
    ['B', '', 'R'],
    ['B', '', 'R']
  ];

  const board = writable<Board>([]);
  const turn = writable<Player>('B');
  const winner = writable<Player | null>(null);
  const selected = writable<{ x: number; y: number } | null>(null);

  const validMoves = derived([board, selected], ([$board, $selected]) => {
    if (!$selected) return [];
    const moves: { x: number; y: number }[] = [];
    for (let y = 0; y < size; y++) {
      for (let x = 0; x < size; x++) {
        if ($board[y][x] === '' && isConnectedViaSymmetricLine($selected.x, $selected.y, x, y)) {
          moves.push({ x, y });
        }
      }
    }
    return moves;
  });

  $: validMovesVal = $validMoves;

  function resetGame() {
    board.set(structuredClone(initialBoard));
    turn.set('B');
    winner.set(null);
    selected.set(null);
  }

  function isWinningMove(bd: Board, player: Player): boolean {
    const winLine = player.repeat(3);
    const lines: string[] = [];
    for (let i = 0; i < size; i++) {
      lines.push(bd[i].join(''));
      lines.push(bd.map(row => row[i]).join(''));
    }
    lines.push([bd[0][0], bd[1][1], bd[2][2]].join(''));
    lines.push([bd[0][2], bd[1][1], bd[2][0]].join(''));
    return lines.some(line => line === winLine);
  }

  function isConnectedViaSymmetricLine(fromX: number, fromY: number, toX: number, toY: number): boolean {
    const connections = new Set([
      '0,0-1,0', '0,0-1,1', '0,0-0,1',
      '0,1-0,0', '0,1-0,2', '0,1-1,1',
      '0,2-1,2', '0,2-1,1', '0,2-0,1',
      '1,0-0,0', '1,0-2,0', '1,0-1,1',
      '1,1-0,0', '1,1-0,1', '1,1-0,2',
      '1,1-1,0',             '1,1-1,2',
      '1,1-2,0', '1,1-2,1', '1,1-2,2',
      '1,2-0,2', '1,2-2,2', '1,2-1,1',
      '2,0-1,0', '2,0-2,1', '2,0-1,1',
      '2,1-2,0', '2,1-2,2', '2,1-1,1',
      '2,2-1,2', '2,2-2,1', '2,2-1,1'
    ]);

    const key = `${fromX},${fromY}-${toX},${toY}`;
    const reverseKey = `${toX},${toY}-${fromX},${fromY}`;
    return connections.has(key) || connections.has(reverseKey);
  }

  function handleClick(x: number, y: number) {
    board.update(bd => {
      if ($winner) return bd;
      const current = $turn;
      const sel = $selected;

      if (sel && bd[sel.y][sel.x] === current) {
        if (sel.x === x && sel.y === y) {
          selected.set(null);
        } else if (bd[y][x] === current) {
          selected.set({ x, y });
        } else if (isConnectedViaSymmetricLine(sel.x, sel.y, x, y) && bd[y][x] === '') {
          bd[y][x] = current;
          bd[sel.y][sel.x] = '';
          selected.set(null);
          if (isWinningMove(bd, current)) {
            if (!matchesInitialPattern(bd, current)) {
              winner.set(current);
            }
          } else {
            turn.set(current === 'B' ? 'R' : 'B');
          }
        }
      } else if (bd[y][x] === current) {
        selected.set({ x, y });
      } else {
        selected.set(null);
      }
      return bd;
    });
  }

  function matchesInitialPattern(bd: Board, player: Player): boolean {
    const initial = structuredClone(initialBoard);
    for (let y = 0; y < size; y++) {
      for (let x = 0; x < size; x++) {
        if (bd[y][x] === player && initial[y][x] !== player) {
          return false;
        }
      }
    }
    return true;
  }

  onMount(() => resetGame());
</script>

<div class="min-h-screen bg-gradient-to-br from-yellow-100 via-orange-200 to-red-100 py-10 px-4 flex flex-col items-center justify-start">
  <h1 class="text-4xl font-extrabold text-center text-orange-800 drop-shadow-md mb-6">Lelempengan</h1>

  {#if $winner}
    <p class="text-xl text-center text-green-600 font-semibold mb-3">Pemenang: {$winner === 'B' ? '🔵 Biru' : '🔴 Merah'}</p>
    <button on:click={resetGame} class="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition">Main Lagi</button>
  {:else}
    <p class="text-lg text-center text-gray-800 font-medium mb-3">Giliran: {$turn === 'B' ? '🔵 Biru' : '🔴 Merah'}</p>
  {/if}

  <div class="relative w-[300px] sm:w-[360px] aspect-square">
    <div class="grid grid-cols-3 gap-2 absolute inset-0">
      {#each $board as row, y}
        {#each row as cell, x}
          <div
            class="rounded-lg shadow-lg flex items-center justify-center text-3xl font-bold border border-gray-500 cursor-pointer transition-all duration-200 select-none"
            on:click={() => handleClick(x, y)}
            class:selected={$selected?.x === x && $selected?.y === y}
            class:bg-blue-400={cell === 'B'}
            class:bg-red-400={cell === 'R'}
            class:bg-green-200={!cell && validMovesVal.some(m => m.x === x && m.y === y)}
            class:bg-white={!cell && !validMovesVal.some(m => m.x === x && m.y === y)}
          >
            {cell === 'B' ? '🔵' : cell === 'R' ? '🔴' : ''}
          </div>
        {/each}
      {/each}
    </div>
  </div>
</div>

<style>
  .selected {
    outline: 3px dashed #000;
    outline-offset: 2px;
  }
</style>
