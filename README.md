![tic tac toe](https://github.com/user-attachments/assets/98901a3b-0e57-459b-b7b7-063acfff0f9a)
% Define winning positions
win(Player, Board) :-
    RowWin = [Player, Player, Player],
    (Board = [RowWin, _, _];
     Board = [_, RowWin, _];
     Board = [_, _, RowWin];
     Board = [[Player, _, _], [Player, _, _], [Player, _, _]];
     Board = [[_, Player, _], [_, Player, _], [_, Player, _]];
     Board = [[_, _, Player], [_, _, Player], [_, _, Player]];
     Board = [[Player, _, _], [_, Player, _], [_, _, Player]];
     Board = [[_, _, Player], [_, Player, _], [Player, _, _]]).

% Display the board
print_board([]).
print_board([Row|Rest]) :-
    write(Row), nl,
    print_board(Rest).

% Replace element at a specific position
replace([_|T], 0, X, [X|T]).
replace([H|T], I, X, [H|R]) :-
    I > 0,
    I1 is I - 1,
    replace(T, I1, X, R).

replace2D([Row|Rest], 0, Col, X, [NewRow|Rest]) :-
    replace(Row, Col, X, NewRow).
replace2D([Row|Rest], RowIdx, Col, X, [Row|NewRest]) :-
    RowIdx > 0,
    R1 is RowIdx - 1,
    replace2D(Rest, R1, Col, X, NewRest).

% Get a move from the player
get_move(Row, Col) :-
    write('Enter row (0-2): '), read(Row),
    write('Enter col (0-2): '), read(Col).

% Check if cell is empty
valid_move(Board, Row, Col) :-
    nth0(Row, Board, BoardRow),
    nth0(Col, BoardRow, Cell),
    var(Cell).

% Game loop
play(Board, Player) :-
    print_board(Board),
    (win(x, Board) -> write('Player x wins!'), nl;
     win(o, Board) -> write('Player o wins!'), nl;
     flatten(Board, Flat), \+ member(_, Flat) -> write('It\'s a draw!'), nl;
     write('Player '), write(Player), write(' turn.'), nl,
     get_move(Row, Col),
     (valid_move(Board, Row, Col) ->
         replace2D(Board, Row, Col, Player, NewBoard),
         switch_player(Player, NextPlayer),
         play(NewBoard, NextPlayer)
     ;
         write('Invalid move! Try again.'), nl,
         play(Board, Player))).

% Switch player
switch_player(x, o).
switch_player(o, x).

% Start the game
start :-
    Board = [[_, _, _],
             [_, _, _],
             [_, _, _]],
    play(Board, x).
